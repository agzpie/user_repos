<template>
    <div>
        <hr>
        <p class="repo-name">
            {{ repositoryName }}
            &ensp;
            <svg width="15" height="13" viewBox="0 -1 15 14" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M7.5 1L9.33389 4.97587L13.6819 5.49139L10.4673 8.46413L11.3206 12.7586L7.5 10.62L3.6794 12.7586L4.5327 8.46413L1.31813 5.49139L5.66611 4.97587L7.5 1Z" stroke="#666666" stroke-width="1.2" stroke-linejoin="round"/>
            </svg>

            <span class="star"> {{ stars }}</span>
        </p>
        <p class="desc">{{ description }}</p>
    </div>
</template>

<script>
export default {
    props: [
        'repositoryName',
        'description',
        'stars'
    ],
}
</script>

<style>
hr {
    border-top: 0.6px solid #cccccc;
}
.repo-name {
    font-weight: 600;
    font-size: 1rem;
    color: #0862CC;
    /* TODO make repository name a link */
}

.svg {
    vertical-align: middle;;
}

.star {
    vertical-align: middle;
    font-size: 0.8rem;
    font-weight: 500;
    color: #595959;
    /* TODO make star golden on hover */
}

.desc {
    font-size: 0.8rem;
}
</style>