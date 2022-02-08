<template>
	<div>
		<h1>Hello! It is task-table</h1>
		<custom-table></custom-table>
	</div>
</template>

<script>

	import CustomTable from '../components/CustomTable.vue'
	export default {
  components: { CustomTable },
		name: 'TaskPage',
	}
</script>

