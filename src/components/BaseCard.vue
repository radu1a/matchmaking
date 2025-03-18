<template>
    <div class="card-container">
        <div class="card-header">
            <div class="master-container">
                <div class="master-img-container">
                    <img :src="'/' + master.country + '.png'" alt="flag">
                </div>
                <div class="master-name">
                    {{ master.name }}
                </div>
            </div>
        </div>
        <div class="card-body">
            <div class="list-container">
                <div class="list-item" v-for="opp of sortedOpps">
                    <div class="img-container">
                        <img :src="'/' + opp.country + '.png'" alt="flag">
                    </div>
                    <div class="team-name">
                        {{ opp.name }}
                    </div>
                    <div class="fixture-container">
                        {{ (opp.homeOrAway === undefined) ? "u" : (opp.homeOrAway ? "Home" : "Away") }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ["master", "opps"],
    data() {
        return {

        }
    },
    computed: {
        sortedOpps() {
            return this.opps.sort((a, b) => a.homeOrAway <= b.homeOrAway  )
        }
    }
}
</script>

<style scoped>
.card-header {
    height: 15%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #8a2be2;
}
.card-body {
    height: 85%;
    width: 100%;
}
.card-container {
    flex-grow: 1;
    display: flex;
    align-items: center;
    flex-direction: column;
    
}
.list-container{
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
}
.list-item {
    border-top-style: solid;
    border-top-width: 1px;
    border-top-color: white;
    display: flex;
    align-items: center;
    height: 25px;
    flex-grow: 1;
}
.master-container {
    font-size: large;
    height: 100%;
    width: 100%;
    display: flex;
    align-items: center;
    padding-left: 5px;
}
.master-name {
    height: fit-content;
    margin-left: 35px;
}
.team-name {
    margin-left: 15px;
}
.img-container {
    height: 80%;
    max-height: 25px;
    width: 25px;
    max-width: 25px;
    overflow: hidden;
    margin-left: 4px;
}
.master-img-container {
    height: 30;
    max-height: 30px;
    width: 35px;
    max-width: 35px;
    overflow: hidden;
    margin-left: 4px;
}
img {
    max-width: 100%;
    max-height: 100%;
    width: 100%;
    height: 100%;
    object-fit: fill;
    display: block;
}
.fixture-container {
    margin-left: auto;
    margin-right: 5px;
}
</style>