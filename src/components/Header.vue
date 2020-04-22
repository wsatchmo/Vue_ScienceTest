<template>
    <div>
        <b-nav tabs align="center" style="margin-bottom: 2rem;">
            <b-nav-item href="/Vue_ScienceTest/"><b-icon-arrow-repeat/> Reset</b-nav-item>
            <b-nav-item disabled>
                Counter: {{numCorrect}}/{{numTotal}} 
                <b-icon-award
                    v-if="numCorrect >= 15 && numCorrect < 20"
                />
                <b-icon-trophy
                    v-if="numCorrect === 20"
                />
            </b-nav-item>
            <b-nav-item href="https://github.com/wsatchmo" target="_blank">Github <b-icon-code-slash/></b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal'
    ]
}
</script>