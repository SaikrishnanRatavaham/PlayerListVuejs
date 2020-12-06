<template>
   <div >
      <b-container class="bv-playerlist-row">
        <b-row >
        <b-col style="margin-top: 32px!important;">	
           <h1>Players List </h1>
       </b-col>
  </b-row>
           <b-row >
        <b-col >
           <div class="separator mb-3"></div>	
       <b-form-input  type="text" v-model="searchQuery" placeholder="Search">
       </b-form-input>
       </b-col>
  </b-row>
         <b-row >
            <b-col lg="6" sm="6" md='6' v-for="playerDetails in resultQuery" :key="playerDetails.id" >
               <div class="mt-4" :filter="searchQuery">
                  <div  class=" card mb-3  d-flex flex-row flex-xl-row flex-sm-row flex-md-row " >
                     <div
                        v-if="playerDetails.playerList.Id != 99690" style="border-right: 2px solid #f8f8f8;">
                        <img   v-bind:src="require('../assets/player-images/' + playerDetails.playerList.Id + '.jpg')"  :alt="playerDetails.playerList.Id" class= " card-img-left">
                     </div>
                     <div class="card-body" style="margin-top: 44px;">
                        <h5  class=" font-weight-bold"> {{(playerDetails.playerList.PFName) ? playerDetails.playerList.PFName : '-'}}</h5>
                        <!-- <h5  class="d-inline ">Cristiano Ronaldo </h5> -->
                        <div class="d-flex flex-column ">
                           <div class="d-flex flex-row ">
                              <p class=" mr-2 card-text font-weight-semibold ">Positions :</p>
                              <p class="card-text font-weight-semibold">{{(playerDetails.playerList.SkillDesc) ? playerDetails.playerList.SkillDesc : '-'}}</p>
                           </div>
                           <div class="d-flex flex-row  ">
                              <p class=" mr-2 card-text font-weight-semibold ">Base salary :</p>
                              <p class="lead text-center">${{(playerDetails.playerList.Value) ? playerDetails.playerList.Value : '-'}} </p>
                           </div>
                           <div class="  d-flex flex-column "  v-if="playerDetails.UpComingMatchesList.list.CCode"  > 
                              <p class="mb-2 font-weight-bold" >Upcoming match</p>
                              <h5 class="d-inline "> {{(playerDetails.UpComingMatchesList.list.CCode) ? playerDetails.UpComingMatchesList.list.CCode : 'No match'}} VS {{(playerDetails.UpComingMatchesList.list.VsCCode) ? playerDetails.UpComingMatchesList.list.VsCCode : '-'}}</h5>
                           </div>
                              <div class="  d-flex flex-column "  v-if="!playerDetails.UpComingMatchesList.list.CCode"  > 
                              <p class="mb-2 font-weight-bold" >No upcoming match</p>
                               <h5 class="d-inline "> </h5>
                           </div>
                        </div>
                        <p class="mb-0 text-muted"  v-if="playerDetails.UpComingMatchesList.list.CCode" > {{(playerDetails.UpComingMatchesList.date) ? playerDetails.UpComingMatchesList.date : '-'}}</p>
                     </div>
                  </div>
               </div>
            </b-col>
         </b-row>
      </b-container>
   </div>
</template>
<script >
 import axios from "axios";
import moment from "moment";
const URL = "https://api.npoint.io/d6bd0efc05639084eb17/";
export default {
    name: "PlayerList",
    data() {
        return {
            playerListData: [],
            searchQuery: null,
            listPlayerData: [],
            dateMatch: ""
        };
    },

    computed: {
        resultQuery() {
            if (this.searchQuery) {
                console.log(this.searchQuery);
                return this.listPlayerData.filter(item => {
                    return this.searchQuery
                        .toLowerCase()
                        .split(" ")
                        .every(v => item.playerList.PFName.toLowerCase().includes(v) || item.playerList.TName.toLowerCase().includes(v));
                });
            } else {
                return this.listPlayerData;
            }
        }
    },
    created() {
        axios
            .get(URL)
            .then(response => {
                this.playerListData = response.data.playerList;
                this.playerListData.sort((a, b) => (a.value > b.value ? 1 : -1));
                for (var i = 0; i < this.playerListData.length; i++) {
                    for (var j = 0; j < this.playerListData[i].UpComingMatchesList.length; j++) {
                        this.listPlayerData.push({
                            playerList: this.playerListData[i],
                            UpComingMatchesList: {
                                list: this.playerListData[i].UpComingMatchesList[j],
                                date: this.playerListData[i].UpComingMatchesList[j].MDate ? moment
                                    .utc(
                                        this.playerListData[i].UpComingMatchesList[j].MDate,
                                        "MM-DD-YYYY"
                                    )
                                    .local()
                                    .format("DD-MM-YYYY h:mm:ss a") :
                                    "-"
                            }
                        });
                    }
                }
            })
            .catch(err => {
                console.log(err);
            });
    }
}; 
</script>

<style scoped>
   h3 {
   margin: 40px 0 0;
   }
   ul {
   list-style-type: none;
   padding: 0;
   }
   li {
   display: inline-block;
   margin: 0 10px;
   }
   a {
   color: #42b983;
   }
   p {
   font-size: .85rem;
   line-height: 1.3rem;
   font-family: Nunito,sans-serif;
   }
   h1 {
   font-size: 1.75rem;
   padding-bottom: 10px;
   display: inline-block;
   }
   h5 {
   font-size: 1.1rem;
   }
   .list-item-heading {
   font-size: 1rem;
   }
   .font-weight-bold {
   font-weight: 700!important;
   }
   .text-muted {
   color: #909090!important;
   }
   h6 {
   font-size: 1rem;
   }
   .lead {
   font-size: 23px;
   font-weight: 300;
   color: #145388;
   }
   .lead {
   color: #145388;
   }
   .font-weight-semibold {
   font-weight: 600;
   color: #8f8f8f;
   }
   .card-text {
   color: #8f8f8f;
   height: 15px;
   line-height: 26px;
   }

   .separator {
    border-bottom: 1px solid #d7d7d7;
}
</style>