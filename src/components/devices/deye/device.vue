<template>
	<div class="device-batterx">
		<openwb-base-heading>
			Einstellungen für Deye
			<span class="small">(Modul: {{ $options.name }})</span>
		</openwb-base-heading>
		<openwb-base-text-input
			title="IP oder Hostname"
			subtype="host"
			required
			:model-value="configuration.ip_address"
			@update:model-value="
				updateConfiguration($event, 'configuration.ip_address')
			"
		/>
		<openwb-base-number-input
			title="Port"
			required
			:model-value="configuration.port"
			@update:model-value="
				updateConfiguration($event, 'configuration.port')
			"
		/>
		<openwb-base-select-input
			title="Bauform"
			notSelected="Bitte auswählen"
			:options="[
				{
					value: 'three_phase',
					text: 'dreiphasiger Wechselrichter',
				},
				{
					value: 'single_phase_string',
					text: 'einphasiger String-Wechselrichter (ungetestet, Rückmeldung über Support erwünscht)',
				},
				{
					value: 'single_phase_hybrid',
					text: 'einphasiger Hybrid-Wechselrichter (ungetestet, Rückmeldung über Support erwünscht)',
				},
			]"
			:model-value="configuration.device_type"
			@update:model-value="
				updateConfiguration($event, 'configuration.device_type')
			"
		>
			<template #help>
				Zwischen Version 1.7 und 1.10 hat sich das Register für die
				Speicherleistung geändert. Wenn die Speicherleistung nicht
				korrekt ausgelesen wird, bitte die andere Version testen.
			</template>
		</openwb-base-select-input>
	</div>
</template>

<script>
export default {
	name: "DeviceDeye",
	emits: ["update:configuration"],
	props: {
		configuration: { type: Object, required: true },
		deviceId: { default: undefined },
	},
	methods: {
		updateConfiguration(event, path = undefined) {
			this.$emit("update:configuration", { value: event, object: path });
		},
	},
};
</script>
