<template>
  <main class="main-container">
   <Header></Header>
   <section class="block list-campaign-wrapper">
     <div class="list-campaign-wrapper__header display-flex flex-align-center">
       <div class="icon">
         <span></span>
         <span></span>
         <span></span>
       </div>
       <p>Campaign List</p>
       <button class="btn create-campaign-btn" @click="showCreateCampaignPopup">+ Create New</button>
     </div>
     <div class="list-campaign-wrapper__campaign-details block display-flex">
       <div class="flex-2">
         <div class="list-campaign-wrapper__campaign-details__list">
           <div class="campaign display-flex flex-justify-content-space-between cursor-pointer flex-align-center" v-for="(campaign, index) in campaignsArray" :key="index" :class="{'active': campaign.isActive}" @click='getCampaignDetails(campaign)'>
             <div class="name-type-wrapper display-flex flex-align-center">
               <div class="index">
                 <span>{{index+1}}</span>
               </div>
               <div class="name">
                 <h3>{{campaign.name}} <span v-if="campaign.type === 'eMail'">Email</span><span v-if="campaign.type === 'pushNotification'"> Push Notification</span><span v-if="campaign.type === 'sms'"> SMS</span></h3>
                 <h4>Created at {{campaign.createdAt}}</h4>
               </div>
             </div>
             <div class="acions display-flex text-center">
               <div class="flex-1">
                 <a class="cursor-pointer" @click='pauseCampaign(campaign)'>
                   <i class="fa fa-pause-circle" aria-hidden="true"></i>
                   <h4>Pause</h4>
                  </a>
               </div>
               <div class="flex-1">
                 <a class="cursor-pointer">
                   <i class="fa fa-comment" aria-hidden="true"></i>
                   <h4>Comment</h4>
                  </a>
               </div>
               <div class="flex-1">
                 <a class="cursor-pointer">
                   <i class="fa fa-pencil" aria-hidden="true"></i>
                   <h4>Rename</h4>
                  </a>
               </div>
               <div class="flex-1">
                 <a class="cursor-pointer" @click='deleteCampaign(campaign)'>
                   <i class="fa fa-trash-o" aria-hidden="true"></i>
                   <h4>Delete</h4>
                  </a>
               </div>
             </div>
           </div>
          </div>
       </div>
       <div class="flex-1 display-flex">
         <div class="list-campaign-wrapper__campaign-details__history block">
           <div class="title">
             <h3> <i class="fa fa-history" aria-hidden="true"></i> History</h3>
             <h5 v-if="selectedCampaign.name">{{selectedCampaign.name}} - <span v-if="selectedCampaign.type === 'eMail'"> Emails</span><span v-if="selectedCampaign.type === 'pushNotification'"> Push Notification</span><span v-if="selectedCampaign.type === 'sms'"> SMS</span></h5>
             <h5 v-if="!selectedCampaign.name">Please select a campaign to get the details</h5>
           </div>
           <div class="details">
             <ul v-if='selectedCampaign.comments && selectedCampaign.comments.length > 1'>
               <li v-for="(comment, index) in selectedCampaign.comments" :key="index">
                 <span class="icon display-flex" :class="{'paused': comment.historyType === 'Paused', 'created': comment.historyType === 'Campaign Created'}">
                   <i class="fa fa-plus" aria-hidden="true" v-if="comment.historyType==='Campaign Created'"></i>
                   <i class="fa fa-comment" aria-hidden="true" v-if="comment.historyType==='Comment Added'"></i>
                   <i class="fa fa-pencil" aria-hidden="true" v-if="comment.historyType==='Comment Renamed'"></i>
                   <i class="fa fa-pause" aria-hidden="true" v-if="comment.historyType==='Paused'"></i>
                 </span>
                 <div class="comments-details">
                   <h5>{{comment.historyType}}</h5>
                   <h6>by <span>{{comment.userName}}</span></h6>
                   <p v-if="comment.message">{{comment.message}}</p>
                 </div>
                </li>
             </ul>
             <h4 v-if='selectedCampaign.comments && selectedCampaign.comments.length < 1' class="text-center">History Not Found</h4>
           </div>
         </div>
       </div>
     </div>
    <div class="create-campaign-wrapper display-flex" :class="{'active': showHideCreatePopup}">
      <div class="form margin-auto">
        <h4 class="text-center">Create Campaign</h4>
        <div class="form-group margin-bottom-10px">
          <label>Name</label>
          <input type="text" placeholder="Please enter the name" v-model="campaignName"/>
        </div>
        <div class="form-group text-center">
          <button type="button" class="btn" @click="createCampaign">Create</button>
        </div>
        <a class="cursor-pointer close-btn" @click="hideCreateCampaignPopup">X</a>
      </div>
      </div>
   </section>
  </main>
</template>

<script>
import Header from '@/components/header/header';

export default {
  name: 'HelloWorld',
  components: {
    Header,
  },
  data() {
    return {
      campaignsArray: [{
        id: 100,
        name: 'Campaign 1',
        type: 'eMail',
        comments: [{
          historyType: 'Campaign Created',
          userName: 'Chirag',
        },
        {
          historyType: 'Comment Added',
          userName: 'Chirag',
          message: 'Good Luck',
        },
        {
          historyType: 'Comment Renamed',
          userName: 'Chirag',
        },
        {
          historyType: 'Paused',
          userName: 'Nithin',
        }],
        history: [],
        createdAt: '2:30 pm',
        isActive: false,
      },
      {
        id: 101,
        name: 'Campaign 2',
        type: 'pushNotification',
        comments: [{
          historyType: 'Campaign Created',
          userName: 'Sandeep',
        },
        {
          historyType: 'Comment Added',
          userName: 'Chirag',
          message: 'Good Luck Sandy',
        },
        {
          historyType: 'Comment Renamed',
          userName: 'Chirag',
        },
        {
          historyType: 'Paused',
          userName: 'Nithin',
        },
        {
          historyType: 'Comment Added',
          userName: 'Sandeep',
          message: 'Resume kar de bhai',
        },
        {
          historyType: 'Paused',
          userName: 'Nithin',
        }],
        history: [],
        createdAt: '2:30 pm',
        isActive: false,
      },
      {
        id: 103,
        name: 'Campaign 3',
        type: 'sms',
        comments: [],
        history: [],
        createdAt: '2:30 pm',
        isActive: false,
      },
      {
        id: 104,
        name: 'Campaign 4',
        type: 'eMail',
        comments: [],
        history: [],
        createdAt: '2:30 pm',
        isActive: false,
      },
      {
        id: 105,
        name: 'Campaign 5',
        type: 'pushNotification',
        comments: [],
        history: [],
        createdAt: '2:30 pm',
        isActive: false,
      },
      {
        id: 106,
        name: 'Campaign 6',
        type: 'sms',
        comments: [],
        history: [],
        createdAt: '2:30 pm',
        isActive: false,
      },
      {
        id: 107,
        name: 'Campaign 7',
        type: 'eMail',
        comments: [],
        history: [],
        createdAt: '2:30 pm',
        isActive: false,
      }],
      selectedCampaign: {},
      // flag to show hide the create campaign popup
      showHideCreatePopup: false,
      // variable to store the campaign name
      campaignName: '',
    };
  },
  methods: {
    // function to populate the campaign history
    getCampaignDetails(campaignObj) {
      this.selectedCampaign = campaignObj;
      this.highlightSelectedCampaign(campaignObj);
    },
    // function to highlight the selected campaign
    highlightSelectedCampaign(campaignObj) {
      // eslint-disable-next-line
      this.campaignsArray.map((campaign) => {
        const tempCampaign = campaign;
        if (tempCampaign.id === campaignObj.id) {
          tempCampaign.isActive = true;
        } else {
          tempCampaign.isActive = false;
        }
      });
    },
    // function to pause a campaign
    pauseCampaign(campaignObj) {
      this.$swal({
        title: 'Pause Campaign',
        html: `Do you want to pause <strong>${campaignObj.name} ?</strong>`,
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Yes, Pause it!',
      }).then((result) => {
        if (result.value) {
          this.$swal(
            'Paused!',
            `${campaignObj.name} is paused`,
            'success',
          );
        }
      });
    },
    // function to delete a campaign
    deleteCampaign(campaignObj) {
      this.$swal({
        title: 'Delete Campaign',
        html: `Do you want to delete <strong>${campaignObj.name} ?</strong>`,
        type: 'error',
        showCancelButton: true,
        confirmButtonText: 'Yes, Delete it!',
      }).then((result) => {
        if (result.value) {
          // eslint-disable-next-line
          const newCampaignsArray = this.campaignsArray.filter((campaign) => {
            if (campaign.id !== campaignObj.id) {
              return campaign;
            }
          });
          this.campaignsArray = newCampaignsArray;
          this.$swal(
            'Deleted!',
            `<strong>${campaignObj.name}</strong> is deleted`,
            'success',
          );
        }
      });
    },
    // function to show create campaign
    showCreateCampaignPopup() {
      this.showHideCreatePopup = true;
    },
    // function to hide create campaign
    hideCreateCampaignPopup() {
      this.showHideCreatePopup = false;
    },
    // function to create a campaign
    createCampaign() {
      if (this.campaignName === '') {
        this.$swal(
          'Create Campaign!',
          'Please enter the name',
          'error',
        );
      } else {
        this.campaignsArray.push({
          name: this.campaignName,
          createdAt: new Date(),
          id: Math.ceil(Math.random() * 100),
          isActive: false,
          type: 'eMail',
        });
        this.campaignName = '';
        this.$swal(
          'Created',
          'Campaign Created Successfully!',
          'success',
        );
        this.hideCreateCampaignPopup();
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
</style>
