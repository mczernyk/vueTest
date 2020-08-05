<template>
  <div>

    <h1>Final Score: {{numCorrect}} / {{numTotal}} </h1>

</div>
</template>

<script>

export default {
  props: [
    'numCorrect', 'numTotal'
  ]
}

</script>
