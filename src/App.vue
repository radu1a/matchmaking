<script setup>
import BaseCard from "./components/BaseCard.vue";
</script>

<template>
  <main>
    <div class="pots-container">
      <div class="pot-list">
        <div class="pot" v-for="(group, _, index) in groups">
          <div class="pot-header">
            POT {{ index + 1 }}
          </div>
          <div class="team-list">
            <div class="team-item" v-for="team in group" :key="team.name">
              <img :src="'/' + team.country + '.png'" alt="flag">
              {{ team.name}}
            </div>
          </div>
        </div>
      </div>
      <div class="btn-container">
        <button :class="{ 'create-matches-btn-disabled': resultsGenerated }" class="create-matches-btn" :disabled="resultsGenerated" style="margin-right: 10px"
        :onclick="createButtonClicked">Make the draw!</button>
        <button :class="{ 'create-matches-btn-disabled': !resultsGenerated }" class="create-matches-btn" :disabled="!resultsGenerated" style= "margin-left: 10px"
        :onclick="reset">Reset</button>
      </div>
    </div>
	
	<div class="spinner-container">
		 <div v-if="isLoading" class="loader"></div>
	</div>



    <div class="matches-container">
      <div class="base-card-wrapper" v-for="(oppList, teamName) in matches" :key="teamName">
        <BaseCard :master="getTeamRefByName(teamName)" :opps="oppList"></BaseCard>
      </div>
    </div>
  </main>
</template>

<script>
import cloneDeep from 'lodash/cloneDeep';

export default {
  data() {
    return {
      groups: {
        A: [
          {
            name: "Man City",
            country: "england",
            group: "A",
          },
          {
            name: "Bayern Munich",
            country: "germany",
            group: "A",
          },
          {
            name: "Borussia Dortmund",
            country: "germany",
            group: "A",
          },
          {
            name: "Real Madrid",
            country: "spain",
            group: "A",
          },
          {
            name: "Liverpool",
            country: "england",
            group: "A",
          },
          {
            name: "PSG",
            country: "france",
            group: "A",
          },
          
          {
            name: "Inter Milan",
            country: "italy",
            group: "A",
          },
          {
            name: "RB Leipzig",
            country: "germany",
            group: "A",
          },
          {
            name: "Barcelona",
            country: "spain",
            group: "A",
          }
        ],
        B: [
          {
            name: "Bayer Leverkusen",
            country: "germany",
            group: "B",
          },
          {
            name: "Atletico Madrid",
            country: "spain",
            group: "B",
          },
          {
            name: "Atalanta",
            country: "italy",
            group: "B",
          },
          {
            name: "Benfica",
            country: "portugal",
            group: "B",
          },
          {
            name: "Juventus",
            country: "italy",
            group: "B",
          },
          {
            name: "Arsenal",
            country: "england",
            group: "B",
          },
          {
            name: "Club Brugge",
            country: "belgium",
            group: "B",
          },
          {
            name: "Shakhtar Donetsk",
            country: "ukraine",
            group: "B",
          },
          {
            name: "AC Milan",
            country: "italy",
            group: "B",
          }
        ],
        C: [
        {
            name: "Feyenoord",
            country: "netherlands",
            group: "C",
          },
          {
            name: "Sporting CP",
            country: "portugal",
            group: "C",
          },
          {
            name: "Lille",
            country: "france",
            group: "C",
          },
          {
            name: "PSV",
            country: "netherlands",
            group: "C",
          },
          {
            name: "PAOK",
            country: "greece",
            group: "C",
          },
          {
            name: "Young Boys",
            country: "switzerland",
            group: "C",
          },
          {
            name: "Celtic",
            country: "scotland",
            group: "C",
          },
          {
            name: "Galatasaray",
            country: "turkey",
            group: "C",
          },
          {
            name: "Union SG",
            country: "belgium",
            group: "C",
          },
        ],
        D: [
          {
            name: "Midtylland",
            country: "denmark",
            group: "D",
          },
          {
            name: "AS Monaco",
            country: "france",
            group: "D",
          },
          {
            name: "Sparta Prague",
            country: "czech-rep",
            group: "D",
          },
          {
            name: "Aston Villa",
            country: "england",
            group: "D",
          },
          {
            name: "Bologna",
            country: "italy",
            group: "D",
          },
          {
            name: "Girona",
            country: "spain",
            group: "D",
          },
          {
            name: "Stuttgart",
            country: "germany",
            group: "D",
          },
          {
            name: "Sturm Graz",
            country: "austria",
            group: "D",
          },
          {
            name: "Stade Brestois",
            country: "france",
            group: "D",
          },
        ],
      },
      groupsCopy: {
        A: [
          {
            name: "Man City",
            country: "england",
            group: "A",
          },
          {
            name: "Bayern Munich",
            country: "germany",
            group: "A",
          },
          {
            name: "Borussia Dortmund",
            country: "germany",
            group: "A",
          },
          {
            name: "Real Madrid",
            country: "spain",
            group: "A",
          },
          {
            name: "Liverpool",
            country: "england",
            group: "A",
          },
          {
            name: "PSG",
            country: "france",
            group: "A",
          },
          
          {
            name: "Inter Milan",
            country: "italy",
            group: "A",
          },
          {
            name: "RB Leipzig",
            country: "germany",
            group: "A",
          },
          {
            name: "Barcelona",
            country: "spain",
            group: "A",
          }
        ],
        B: [
          {
            name: "Bayer Leverkusen",
            country: "germany",
            group: "B",
          },
          {
            name: "Atletico Madrid",
            country: "spain",
            group: "B",
          },
          {
            name: "Atalanta",
            country: "italy",
            group: "B",
          },
          {
            name: "Benfica",
            country: "portugal",
            group: "B",
          },
          {
            name: "Juventus",
            country: "italy",
            group: "B",
          },
          {
            name: "Arsenal",
            country: "england",
            group: "B",
          },
          {
            name: "Club Brugge",
            country: "belgium",
            group: "B",
          },
          {
            name: "Shakhtar Donetsk",
            country: "ukraine",
            group: "B",
          },
          {
            name: "AC Milan",
            country: "italy",
            group: "B",
          }
        ],
        C: [
        {
            name: "Feyenoord",
            country: "netherlands",
            group: "C",
          },
          {
            name: "Sporting CP",
            country: "portugal",
            group: "C",
          },
          {
            name: "Lille",
            country: "france",
            group: "C",
          },
          {
            name: "PSV",
            country: "netherlands",
            group: "C",
          },
          {
            name: "PAOK",
            country: "greece",
            group: "C",
          },
          {
            name: "Young Boys",
            country: "switzerland",
            group: "C",
          },
          {
            name: "Celtic",
            country: "scotland",
            group: "C",
          },
          {
            name: "Galatasaray",
            country: "turkey",
            group: "C",
          },
          {
            name: "Union SG",
            country: "belgium",
            group: "C",
          },
        ],
        D: [
          {
            name: "Midtylland",
            country: "denmark",
            group: "D",
          },
          {
            name: "AS Monaco",
            country: "france",
            group: "D",
          },
          {
            name: "Sparta Prague",
            country: "czech-rep",
            group: "D",
          },
          {
            name: "Aston Villa",
            country: "england",
            group: "D",
          },
          {
            name: "Bologna",
            country: "italy",
            group: "D",
          },
          {
            name: "Girona",
            country: "spain",
            group: "D",
          },
          {
            name: "Stuttgart",
            country: "germany",
            group: "D",
          },
          {
            name: "Sturm Graz",
            country: "austria",
            group: "D",
          },
          {
            name: "Stade Brestois",
            country: "france",
            group: "D",
          },
        ],
      },
      matches: {},
      nrSameCountryTeamsAllowed: 2,
      crossMatchesOf4: {
        "AB": [],
        "AC": [],
        "AD": [],
        "BC": [],
        "BD": [],
        "CD": [],
      },
      resultsGenerated: false,
	  isLoading: false
    }
  },
  methods: {
    reset() {
      this.resultsGenerated = false
	  
      this.groups = cloneDeep(this.groupsCopy)
	  
      this.matches = {}
      this.crossMatchesOf4 = {
        "AB": [],
        "AC": [],
        "AD": [],
        "BC": [],
        "BD": [],
        "CD": [],
      }
    },
    initMatches() {
      this.resultsGenerated = true
	  
      for(const key in this.groups) {
        for (const team of this.groups[key]) {
          this.matches = {
            ...this.matches,
            [team.name]: []
          }
        }
      }
    },

    createTeamList(groups) {
      let list = []
      for (const key in groups) {
        for (const team of groups[key]) {
          list.push(team)
        }
      }
      return list

    },

    createButtonClicked() {
      this.isLoading = true
      
      this.$nextTick(() => {
        setTimeout(() => {
          try {
          this.createMatches();
          this.isLoading = false
          } catch (error) {
          this.reset();
          this.createButtonClicked();  // Retry by calling the function recursively
          }
        }, 0); // Run the heavy computation after the current call stack is clear
      });
    },

    createMatches() {
	  this.shuffle(this.groups)
	  
      this.initMatches()
      let allTeamsList = this.createTeamList(this.groups)
      for (const team of allTeamsList) {
        team["opponents"] = []
      }
      

      ///////////////////////inside////////////////////////////////////


      for (const masterTeam of allTeamsList) {
        for (const team of allTeamsList) {
          if (this.matches[masterTeam.name].length >= 8) {
            break
          }
          if (team.group !== masterTeam.group) {
            continue
          }
          if (this.matches[team.name].length >= 8) {
            continue
          }
          if (team.name === masterTeam.name) {
            continue
          }
          if (team.country === masterTeam.country) {
            continue
          }
          if(this.checkIfTeamExistsInOpps(team.name, this.matches[masterTeam.name]) || this.checkIfTeamExistsInOpps(masterTeam.name, this.matches[team.name])) {
            continue
          }
          if(this.countNrOppsFromGroup(masterTeam.group, this.matches[team.name]) >= 2 || this.countNrOppsFromGroup(team.group, this.matches[masterTeam.name]) >= 2) {
            continue
          }
          let ok = true
          this.matches[masterTeam.name].forEach((teamObj) => {
            if (teamObj.country === team.country) {
              ok = false
            }
          })
          if (!ok) {
            continue
          }

          this.matches[masterTeam.name].push({name: team.name, group: team.group, country: team.country})
          this.matches[team.name].push({name: masterTeam.name, group: masterTeam.group, country: masterTeam.country})

          //populate team.opponents property. we need this to check the opponents before we assign the matches
          masterTeam["opponents"].push(team)
          team["opponents"].push(masterTeam)
        }
      }


      ///////////////////////cross////////////////////////////////////


      for (const allowedCategoryGroup of ["AB", "AC", "AD", "BC", "BD", "CD"]) {
        for (const masterTeam of allTeamsList) {
          if (masterTeam.group !== allowedCategoryGroup[0]) {
            continue
          }
          if (this.existsInMatchOf4InSameCategory(masterTeam, allowedCategoryGroup)) {
            continue
          }

          let insiderGroup = allowedCategoryGroup[0]
          let outsiderGroup = allowedCategoryGroup[1]
          let crossMatchOf4 = [masterTeam]
          let insiderFound = 0
          let outsidersFound = 0

          for (const team of allTeamsList) {
            if (team.group !== allowedCategoryGroup[0] && team.group !== allowedCategoryGroup[1]) {
              continue
            }
            if (this.existsInMatchOf4InSameCategory(team, allowedCategoryGroup)) {
              continue
            }
            if (masterTeam.group > team.group || masterTeam.group === "D") {
              continue
            } 
            if (team.name === masterTeam.name) {
              continue
            }
            if(masterTeam.name === "RB Leipzig" && team.name === "Sttutgart") {
              debugger
            }
            if (team.country === masterTeam.country) {
              continue
            }

            let nrOppsFromSameCountry1 = 0
            let nrOppsFromSameCountry2 = 0
            masterTeam["opponents"].forEach(element => { //verify team.country in masterteam opp list
              if(element.country === team.country) nrOppsFromSameCountry1++
            });
            team["opponents"].forEach(element => { //verify masterteam.country in team opp list
              if(element.country === masterTeam.country) nrOppsFromSameCountry2++
            });
            if(nrOppsFromSameCountry1 >= 2 || nrOppsFromSameCountry2 >= 2) {
              continue
            }
            
            let ok = true
            for (const teamOf4 of crossMatchOf4) {
              if (team.country === teamOf4.country) {
                ok = false
                break
              }
            }
            if (!ok) {
              continue
            }

            if (team.group === masterTeam.group) {
              if (insiderFound < 1) {
                insiderFound++
                crossMatchOf4.push(team)
              }
            } else {
              if (outsidersFound < 2) {
                outsidersFound++
                crossMatchOf4.push(team)
              }
            }

            if (insiderFound === 1 && outsidersFound === 2) {
              this.crossMatchesOf4[insiderGroup + outsiderGroup].push(crossMatchOf4)
              
              //populate team.opponents property. we need this to check the opponents before we assign the matches
              let index = allTeamsList.findIndex(obj => obj.name === crossMatchOf4[0].name)
              allTeamsList[index]["opponents"].push(crossMatchOf4[2])
              index = allTeamsList.findIndex(obj => obj.name === crossMatchOf4[2].name)
              allTeamsList[index]["opponents"].push(crossMatchOf4[0])

              index = allTeamsList.findIndex(obj => obj.name === crossMatchOf4[0].name)
              allTeamsList[index]["opponents"].push(crossMatchOf4[3])
              index = allTeamsList.findIndex(obj => obj.name === crossMatchOf4[3].name)
              allTeamsList[index]["opponents"].push(crossMatchOf4[0])

              index = allTeamsList.findIndex(obj => obj.name === crossMatchOf4[1].name)
              allTeamsList[index]["opponents"].push(crossMatchOf4[2])
              index = allTeamsList.findIndex(obj => obj.name === crossMatchOf4[2].name)
              allTeamsList[index]["opponents"].push(crossMatchOf4[1])

              index = allTeamsList.findIndex(obj => obj.name === crossMatchOf4[1].name)
              allTeamsList[index]["opponents"].push(crossMatchOf4[3])
              index = allTeamsList.findIndex(obj => obj.name === crossMatchOf4[3].name)
              allTeamsList[index]["opponents"].push(crossMatchOf4[1])
              
              for (const teamOf4 of crossMatchOf4) {
                teamOf4[allowedCategoryGroup] = true
              }

              break
            }
          }
        }
      }

      //add the unassigned teams to the last group of 4

      for (const allowedCategoryGroup of ["AB", "AC", "AD", "BC", "BD", "CD"]) {
        for (const masterTeam of allTeamsList) {
          if (masterTeam.group !== allowedCategoryGroup[0] && masterTeam.group !== allowedCategoryGroup[1]) {
                continue
              }
              if (masterTeam[allowedCategoryGroup]) {
                continue
              }
          
          if (masterTeam.group === allowedCategoryGroup[0]) {
                this.crossMatchesOf4[allowedCategoryGroup][3].splice(2, 0, masterTeam)
              }
              else if (masterTeam.group === allowedCategoryGroup[1]) {
                this.crossMatchesOf4[allowedCategoryGroup][3].push(masterTeam)
              }
        }
      if (this.crossMatchesOf4[allowedCategoryGroup][3][5].country === this.crossMatchesOf4[allowedCategoryGroup][3][2].country) {
        let aux = this.crossMatchesOf4[allowedCategoryGroup][3][2]
        this.crossMatchesOf4[allowedCategoryGroup][3][2] = this.crossMatchesOf4[allowedCategoryGroup][3][0]
        this.crossMatchesOf4[allowedCategoryGroup][3][0] = aux
        
      }
      if (this.crossMatchesOf4[allowedCategoryGroup][3][5].country === this.crossMatchesOf4[allowedCategoryGroup][3][1].country) {
        let aux = this.crossMatchesOf4[allowedCategoryGroup][3][1]
        this.crossMatchesOf4[allowedCategoryGroup][3][1] = this.crossMatchesOf4[allowedCategoryGroup][3][0]
        this.crossMatchesOf4[allowedCategoryGroup][3][0] = aux
      }
      
      if (this.crossMatchesOf4[allowedCategoryGroup][3][2].country === this.crossMatchesOf4[allowedCategoryGroup][3][5].country) {
        let aux = this.crossMatchesOf4[allowedCategoryGroup][3][5]
        this.crossMatchesOf4[allowedCategoryGroup][3][5] = this.crossMatchesOf4[allowedCategoryGroup][3][3]
        this.crossMatchesOf4[allowedCategoryGroup][3][3] = aux
      }
      if (this.crossMatchesOf4[allowedCategoryGroup][3][2].country === this.crossMatchesOf4[allowedCategoryGroup][3][4].country) {
        let aux = this.crossMatchesOf4[allowedCategoryGroup][3][4]
        this.crossMatchesOf4[allowedCategoryGroup][3][4] = this.crossMatchesOf4[allowedCategoryGroup][3][3]
        this.crossMatchesOf4[allowedCategoryGroup][3][3] = aux
      }
	  }

      //assign matches based on this.crossMatchesOf4

      for (const groupCategory in this.crossMatchesOf4) {
        this.crossMatchesOf4[groupCategory].forEach((listOfTeams, index) => {
          
          if (index !== 3) {
            this.matches[listOfTeams[0].name].push({name: listOfTeams[2].name, group: listOfTeams[2].group, country: listOfTeams[2].country})
            this.matches[listOfTeams[2].name].push({name: listOfTeams[0].name, group: listOfTeams[0].group, country: listOfTeams[0].country})

            this.matches[listOfTeams[0].name].push({name: listOfTeams[3].name, group: listOfTeams[3].group, country: listOfTeams[3].country})
            this.matches[listOfTeams[3].name].push({name: listOfTeams[0].name, group: listOfTeams[0].group, country: listOfTeams[0].country})

            this.matches[listOfTeams[1].name].push({name: listOfTeams[2].name, group: listOfTeams[2].group, country: listOfTeams[2].country})
            this.matches[listOfTeams[2].name].push({name: listOfTeams[1].name, group: listOfTeams[1].group, country: listOfTeams[1].country})

            this.matches[listOfTeams[1].name].push({name: listOfTeams[3].name, group: listOfTeams[3].group, country: listOfTeams[3].country})
            this.matches[listOfTeams[3].name].push({name: listOfTeams[1].name, group: listOfTeams[1].group, country: listOfTeams[1].country})
          } 
          else {
            this.matches[listOfTeams[0].name].push({name: listOfTeams[3].name, group: listOfTeams[3].group, country: listOfTeams[3].country})
            this.matches[listOfTeams[3].name].push({name: listOfTeams[0].name, group: listOfTeams[0].group, country: listOfTeams[0].country})

            this.matches[listOfTeams[0].name].push({name: listOfTeams[4].name, group: listOfTeams[4].group, country: listOfTeams[4].country})
            this.matches[listOfTeams[4].name].push({name: listOfTeams[0].name, group: listOfTeams[0].group, country: listOfTeams[0].country})

            this.matches[listOfTeams[1].name].push({name: listOfTeams[3].name, group: listOfTeams[3].group, country: listOfTeams[3].country})
            this.matches[listOfTeams[3].name].push({name: listOfTeams[1].name, group: listOfTeams[1].group, country: listOfTeams[1].country})

            this.matches[listOfTeams[1].name].push({name: listOfTeams[5].name, group: listOfTeams[5].group, country: listOfTeams[5].country})
            this.matches[listOfTeams[5].name].push({name: listOfTeams[1].name, group: listOfTeams[1].group, country: listOfTeams[1].country})

            this.matches[listOfTeams[2].name].push({name: listOfTeams[4].name, group: listOfTeams[4].group, country: listOfTeams[4].country})
            this.matches[listOfTeams[4].name].push({name: listOfTeams[2].name, group: listOfTeams[2].group, country: listOfTeams[2].country})

            this.matches[listOfTeams[2].name].push({name: listOfTeams[5].name, group: listOfTeams[5].group, country: listOfTeams[5].country})
            this.matches[listOfTeams[5].name].push({name: listOfTeams[2].name, group: listOfTeams[2].group, country: listOfTeams[2].country})
          }
        });
      }
	  
	  ///check for more than 2 opps from same country
	  for (const key in this.matches) {
		  let countryList = []
		  for (const team of this.matches[key]) {
			  countryList.push(team.country)
		  }
		  for (const country of countryList) {
			  let counter = 0
			  countryList.forEach(el => {
				  if (country === el) counter++
			  })
			  if (counter > 2) throw new Error('More than 2 teams from the same country as opps')
		  }
	  }
	  
	  
      ////////////////////assign home/away////////////////////
	  
		for (const masterTeam of allTeamsList) {
			let aux = true
			for (const team of this.matches[masterTeam.name]) {
				let masterTeamRefFromMatches = this.matches[team.name].find(el => el.name === masterTeam.name)
				if (team["homeOrAway"] !== undefined || masterTeamRefFromMatches["homeOrAway"] !== undefined) {
					continue
				}
				
				masterTeamRefFromMatches["homeOrAway"] = !aux
				team["homeOrAway"] = aux
			  
				if (aux) {
					aux = false
				} 
				else {
				aux = true
				}
				break
			}
		}
		
		for (const groupCategory in this.crossMatchesOf4) {
			for (const listOf4 of this.crossMatchesOf4[groupCategory]) {
				if(listOf4.length === 4) {
					
					let index = this.matches[listOf4[0].name].findIndex(obj => obj.name === listOf4[2].name)
					this.matches[listOf4[0].name][index].homeOrAway = true
					index = this.matches[listOf4[2].name].findIndex(obj => obj.name === listOf4[0].name)
					this.matches[listOf4[2].name][index].homeOrAway = false
					
					index = this.matches[listOf4[0].name].findIndex(obj => obj.name === listOf4[3].name)
					this.matches[listOf4[0].name][index].homeOrAway = false
					index = this.matches[listOf4[3].name].findIndex(obj => obj.name === listOf4[0].name)
					this.matches[listOf4[3].name][index].homeOrAway = true
					
					index = this.matches[listOf4[1].name].findIndex(obj => obj.name === listOf4[2].name)
					this.matches[listOf4[1].name][index].homeOrAway = false
					index = this.matches[listOf4[2].name].findIndex(obj => obj.name === listOf4[1].name)
					this.matches[listOf4[2].name][index].homeOrAway = true
					
					index = this.matches[listOf4[1].name].findIndex(obj => obj.name === listOf4[3].name)
					this.matches[listOf4[1].name][index].homeOrAway = true
					index = this.matches[listOf4[3].name].findIndex(obj => obj.name === listOf4[1].name)
					this.matches[listOf4[3].name][index].homeOrAway = false
				} else {
					let index = this.matches[listOf4[0].name].findIndex(obj => obj.name === listOf4[3].name)
					this.matches[listOf4[0].name][index].homeOrAway = true
					index = this.matches[listOf4[3].name].findIndex(obj => obj.name === listOf4[0].name)
					this.matches[listOf4[3].name][index].homeOrAway = false
					
					index = this.matches[listOf4[0].name].findIndex(obj => obj.name === listOf4[4].name)
					this.matches[listOf4[0].name][index].homeOrAway = false
					index = this.matches[listOf4[4].name].findIndex(obj => obj.name === listOf4[0].name)
					this.matches[listOf4[4].name][index].homeOrAway = true
					
					index = this.matches[listOf4[1].name].findIndex(obj => obj.name === listOf4[3].name)
					this.matches[listOf4[1].name][index].homeOrAway = false
					index = this.matches[listOf4[3].name].findIndex(obj => obj.name === listOf4[1].name)
					this.matches[listOf4[3].name][index].homeOrAway = true
					
					index = this.matches[listOf4[1].name].findIndex(obj => obj.name === listOf4[5].name)
					this.matches[listOf4[1].name][index].homeOrAway = true
					index = this.matches[listOf4[5].name].findIndex(obj => obj.name === listOf4[1].name)
					this.matches[listOf4[5].name][index].homeOrAway = false
					
					index = this.matches[listOf4[2].name].findIndex(obj => obj.name === listOf4[4].name)
					this.matches[listOf4[2].name][index].homeOrAway = true
					index = this.matches[listOf4[4].name].findIndex(obj => obj.name === listOf4[2].name)
					this.matches[listOf4[4].name][index].homeOrAway = false
					
					index = this.matches[listOf4[2].name].findIndex(obj => obj.name === listOf4[5].name)
					this.matches[listOf4[2].name][index].homeOrAway = false
					index = this.matches[listOf4[5].name].findIndex(obj => obj.name === listOf4[2].name)
					this.matches[listOf4[5].name][index].homeOrAway = true
				}
			}
		}
		

      console.log(allTeamsList)
      console.log(this.crossMatchesOf4)
      console.log(this.matches)
    },

    countNrOppsFromGroup(group, oppTeams) {
      let counter = 0
      for (const oppTeam of oppTeams) {
        if(oppTeam.group === group) {
          counter++
        }
      }
      return counter
    },

    checkIfTeamExistsInOpps(teamName, opps) {
      for (const opp of opps) {
        if(opp.name === teamName) return true
      }
      return false
    },

    getTeamRefByName(teamName) {
      for (const key in this.groups) {
        for (const team of this.groups[key]) {
          if (team.name === teamName) {
            return team
          }
        }
      }
      return undefined
    },

    existsInMatchOf4InSameCategory (team, groupCategory) {
      for (const listOf4 of this.crossMatchesOf4[groupCategory]) {
        for (const elOf4 of listOf4) {
          if (elOf4.name === team.name) {
            return true
          }
        }
      }
      return false
    },
	
	  shuffle (groups) {
      for (const group in groups) {
        let rand1 = this.getRandomInt(0, 5)
        let rand2 = this.getRandomInt(5, 9)
        let aux = groups[group][rand1]
        groups[group][rand1] = groups[group][rand2]
        groups[group][rand2] = aux
      }
    },
	
    getRandomInt(min, max) {
      const minCeiled = Math.ceil(min);
      const maxFloored = Math.floor(max);
      return Math.floor(Math.random() * (maxFloored - minCeiled) + minCeiled);
    }
  }
}
</script>

<style scoped>
.loader {
  border: 16px solid #f3f3f3; /* Light grey */
  border-top: 16px solid #3498db; /* Blue */
  border-radius: 50%;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.spinner-container {
	margin-top: 20px;
	width: 100%;
	display: flex;
	justify-content: center
}

.btn-container {
  display: flex;
  justify-content: center;
}
.create-matches-btn {
  background-color: #8a2be2;
  color: white;
  border-radius: 20px;
  border-color: #8a2be2;
  border-style: solid;
  font-size: large;
  padding: 10px;
  padding-top: 15px;
  padding-bottom: 15px;
  margin-top: 20px;
  flex-basis: 25%;
  min-width: 150px;
  max-width: 200px;
}
.create-matches-btn-disabled {
  opacity: 0.33;
  background-color: #8a2be2;
  color: white;
  border-radius: 20px;
  border-color: #8a2be2;
  border-style: solid;
  font-size: large;
  padding: 10px;
  padding-top: 15px;
  padding-bottom: 15px;
  margin-top: 20px;
  flex-basis: 25%;
  min-width: 150px;
  max-width: 200px;
}
button:hover {
  scale: 1.05;
}

.matches-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  margin-bottom: 10px;
}
.team-list {
  position: relative;
  display: flex;
  flex-direction: column;
  height: 85%;
}
.team-item {
  background-color: #3c3c3c;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: white;
  padding-top: 3px;
  padding-bottom: 3px;
  height: 100%;
}
.pot-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.pots-container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  color: white;
  padding: 10px;
}
.pot-header {
  position: relative;
  width: 100%;
  height: 15%;
  background-color: #8a2be2;
  font-size: large;
  display: flex;
  justify-content: center;
  align-items: center;
}
.pot {
  min-width: 150px;
  max-width: 300px;
  flex-basis: 23%;
  min-height: 320px;
  max-height: 320px;
  margin-left: 5px;
  margin-right: 5px;
}

@media (max-width: 700px) {
  .pot {
    flex-basis: 49%;
  }
}
@media (max-width: 630px) {
  .pot {
    flex-basis: 80%;
  }
}

img {
  max-width: 20px;
  max-height: 20px;
}
.base-card-wrapper {
  flex-basis: 15%;
  display: flex;
  min-width: 230px;
  height: 260px;
  margin-top: 20px;
  margin-left: 5px;
  margin-right: 5px;
  padding-bottom: 3px;
  border-style: solid;
  border-color: black;
  border-width: 1px;
  background-color: #9225fe;
  color: white;
  font: bold;
}

@media (max-width: 1460px) {
  .base-card-wrapper {
    flex-basis: 20%;
  }
}
@media (max-width: 970px) {
  .base-card-wrapper {
    flex-basis: 30%;
  }
}
@media (max-width: 730px) {
  .base-card-wrapper {
    flex-basis: 45%;
  }
}
@media (max-width: 490px) {
  .base-card-wrapper {
    flex-basis: 60%;
  }
}
</style>
