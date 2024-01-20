<template>
    <div class="">


        <a class="d-flex justify-content-between list-group-item list-group-item-action" :title="repo.description"
            :href="repo.html_url">
            {{ repo.full_name }}
            <span class="">{{ repo.forks_count }}</span>
        </a>


    </div>
</template>
  
<script>

export default {
    name: 'GitHubRepo',
    props: [
        'repo'
    ]
}
</script>
  
<style></style>
  