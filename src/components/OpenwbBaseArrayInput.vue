<template>
	<div class="form-row mb-1">
		<label v-on:click="toggleHelp" class="col-md-4 col-form-label">
			{{ title }}
			<font-awesome-icon
				v-if="$slots.help"
				:icon="
					showHelp
						? ['fas', 'question-circle']
						: ['far', 'question-circle']
				"
				:class="showHelp ? 'text-info' : ''"
			/>
		</label>
		<div class="col-md-8">
			<div class="form-row">
				<div class="input-group">
					<div class="input-group-prepend">
						<div class="input-group-text">
							<font-awesome-icon
								fixed-width
								:icon="['fas', 'tag']"
							/>
						</div>
					</div>
					<input
						ref="tagInput"
						type="text"
						class="form-control"
						v-model="newTag"
						v-bind="$attrs"
						@keyup.enter="addTag"
					/>
					<div class="input-group-append">
						<div
							class="input-group-text"
							:class="
								newTagValid
									? 'bg-success clickable'
									: 'notClickable'
							"
							@click="addTag"
						>
							<font-awesome-icon
								fixed-width
								:icon="['fas', 'plus']"
							/>
						</div>
					</div>
				</div>
			</div>
			<div class="form-row tagList mt-1">
				<span v-if="value.length == 0" class="noTag">
					<font-awesome-icon :icon="['fas', 'info-circle']" />
					{{ noElementsMessage }}
				</span>
				<span
					class="tag"
					v-for="(tag, index) in value"
					v-bind:key="index"
				>
					<font-awesome-icon :icon="['fas', 'tag']" />
					{{ tag }}
					<font-awesome-icon
						class="clickable"
						:icon="['fas', 'times-circle']"
						@click="removeTag(index)"
					/>
				</span>
			</div>
			<span v-if="showHelp" class="form-row alert alert-info my-1 small">
				<slot name="help"></slot>
			</span>
		</div>
	</div>
</template>

<script>
import { library } from "@fortawesome/fontawesome-svg-core";
import {
	faQuestionCircle as fasQuestionCircle,
	faTag as fasTag,
	faTimesCircle as fasTimesCircle,
	faPlus as fasPlus,
	faInfoCircle as fasInfoCircle,
} from "@fortawesome/free-solid-svg-icons";
import { faQuestionCircle as farQuestionCircle } from "@fortawesome/free-regular-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

library.add(
	fasQuestionCircle,
	farQuestionCircle,
	fasTag,
	fasTimesCircle,
	fasPlus,
	fasInfoCircle,
);

export default {
	name: "OpenwbArrayInput",
	inheritAttrs: false,
	props: {
		title: String,
		modelValue: {
			type: Array,
			default: () => {
				return [];
			},
		},
		noElementsMessage: {
			type: String,
			default: () => {
				return "Keine Elemente zugeordnet.";
			},
		},
	},
	emits: ["update:modelValue"],
	data() {
		return {
			newTag: "",
			showHelp: false,
		};
	},
	computed: {
		value: {
			get() {
				return this.modelValue;
			},
			set(newValue) {
				this.$emit("update:modelValue", newValue);
			},
		},
		newTagValid: {
			get() {
				return (
					this.newTag.length > 0 &&
					this.value.indexOf(this.newTag) == -1
				);
			},
		},
	},
	methods: {
		toggleHelp() {
			this.showHelp = !this.showHelp && this.$slots.help !== undefined;
		},
		addTag() {
			if (this.newTagValid) {
				let tempValue = this.value;
				tempValue.push(this.newTag);
				tempValue.sort();
				this.value = tempValue;
				this.newTag = "";
			}
			this.$refs.tagInput.focus();
		},
		removeTag(index) {
			let tempValue = this.value;
			tempValue.splice(index, 1);
			this.value = tempValue;
		},
	},
	components: {
		FontAwesomeIcon,
	},
};
</script>

<style scoped>
.clickable {
	cursor: pointer;
}

.notClickable {
	cursor: not-allowed;
}

input.invalid,
input:invalid {
	border: 2px solid var(--danger);
}

.tagList {
	background-color: #e9ecef;
	border: 1px solid #ced4da;
	border-radius: 0.25rem;
	padding: 5px 5px 0 5px;
}

.tag,
.noTag {
	border-radius: 10px;
	padding: 2px 5px;
	margin-right: 10px;
	margin-bottom: 5px;
}

.noTag {
	color: var(--gray);
	font-style: italic;
}

.tag {
	background-color: var(--success);
}
</style>
