<template>
  <div id="mail-container">
    <p id="result-text">Results: 0 mail(s)</p>
    <hr v-if="!results.length" class="divider" style="transform: translateY(-10px)">

    <div id="mail-table-container" :style="{justifyContent: results.length ? 'flex-start' : 'center'}">
      <template v-if="!results.length">
        <img src="../assets/png/logo.png" height="100" width="100" class="logo">
      </template>

      <template v-else>
        <div id="mail-table">
          <div id="mail-table-header">
            <div class="table-header-title" style="margin-left: 10px">From</div>
            <div class="table-header-title">To</div>
            <div class="table-header-title" style="width: 35%;">Subject</div>
            <div class="table-header-title" style="width: 15%;">Date</div>
          </div>
          <div class="mail-result-container" v-for="(r,i) in results" :key="i">
            <div class="table-mail-result" @click="r.showMail = !r.showMail">
              <div class="table-mail-result-data" style="width: 25%;">
                <MailIcon class="mail-icon-mobile" />
                {{r.from}}
              </div>

              <div class="table-mail-result-data" style="width: 25%;">
                {{r.recipients.length == 1 ? r.to : `${r.recipients[0]}, ...`}}
                <div v-if="r.recipients.length > 1" class="mail-more-recipients">
                  + {{r.recipients.length - 1}}
                </div>
              </div>

              <div class="table-mail-result-data" style="width: 35%;">
                {{r.subject}}
                
              </div>

              <div class="table-mail-result-data" style="width: 15%;">
                <Clip v-if="r.attachments" class="clip" />
                {{r.date}}
              </div>
            </div>
            <div v-if="r.showMail" class="mail-message">
              {{r.message}}
            </div>
            <div class="mail-divider"></div>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import Clip from "@/assets/svg/icon_clip.svg"
import MailIcon from "@/assets/svg/icon_mail_sp.svg"

export default {
  name: "Mail",
  components: {
    Clip,
    MailIcon
  },
  data() {
    return {
      results: []
    }
  },
  methods: {
    GetAllResults() {
      return {
        status: "200",
        data: [
          {
          from: 'from@example.com',
          to: 'to@example.com',
          subject: '[ HR-888 ] Notice of official accouncement',
          date: "20/6/2020",
          message: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris sodales interdum maximus. Vestibulum nec nunc efficitur, laoreet eros vel, pellentesque odio. Cras at diam molestie, dictum orci sit amet, egestas nulla. Donec sollicitudin diam ac libero luctus volutpat. In auctor sit amet libero id dictum. Suspendisse potenti. Etiam tincidunt odio ac justo lacinia faucibus. Donec tincidunt maximus leo, sit amet cursus felis gravida vitae. Cras et risus sed nulla lacinia egestas. Aliquam venenatis odio a est fermentum faucibus.",
          attachments: 0
        },
        {
          from: 'from@example.com',
          to: 'to@example.com,to@example.com,to@example.com',
          subject: '[ HR-888 ] Notice of official accouncement',
          date: "20/6/2020",
          message: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris sodales interdum maximus. Vestibulum nec nunc efficitur, laoreet eros vel, pellentesque odio. Cras at diam molestie, dictum orci sit amet, egestas nulla. Donec sollicitudin diam ac libero luctus volutpat. In auctor sit amet libero id dictum. Suspendisse potenti. Etiam tincidunt odio ac justo lacinia faucibus. Donec tincidunt maximus leo, sit amet cursus felis gravida vitae. Cras et risus sed nulla lacinia egestas. Aliquam venenatis odio a est fermentum faucibus",
          attachments: 0
        },
        {
          from: 'from@example.com',
          to: 'to@example.com,to@example.com',
          subject: '[ HR-888 ] Notice of official accouncement',
          date: "20/6/2020",
          message: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris sodales interdum maximus. Vestibulum nec nunc efficitur, laoreet eros vel, pellentesque odio. Cras at diam molestie, dictum orci sit amet, egestas nulla. Donec sollicitudin diam ac libero luctus volutpat. In auctor sit amet libero id dictum. Suspendisse potenti. Etiam tincidunt odio ac justo lacinia faucibus. Donec tincidunt maximus leo, sit amet cursus felis gravida vitae. Cras et risus sed nulla lacinia egestas. Aliquam venenatis odio a est fermentum faucibus",
          attachments: 1
        },
        {
          from: 'from@example.com',
          to: 'to@example.com',
          subject: '[ HR-888 ] Notice of official accouncement',
          date: "20/6/2020",
          message: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris sodales interdum maximus. Vestibulum nec nunc efficitur, laoreet eros vel, pellentesque odio. Cras at diam molestie, dictum orci sit amet, egestas nulla. Donec sollicitudin diam ac libero luctus volutpat. In auctor sit amet libero id dictum. Suspendisse potenti. Etiam tincidunt odio ac justo lacinia faucibus. Donec tincidunt maximus leo, sit amet cursus felis gravida vitae. Cras et risus sed nulla lacinia egestas. Aliquam venenatis odio a est fermentum faucibus",
          attachments: 2
        }
        ]
      }
    }
  },
  created() {
    // mocking server request & processing 
    setTimeout(() => {
      this.results = this.GetAllResults().data.map(r => ({...r, showMail: false, recipients: r.to.split(',')}));
    }, 1000)
  }
}
</script>

<style>
#mail-container {
  border: 1px solid black;
  display: flex;
  flex-direction: column;
  max-width: 1340px;
}

#result-text {
  font-weight: bold;
  font-family: Arial, Helvetica, sans-serif;
  color: #666;
}

.divider {
  width: 100%;
  height: 1px;
  color: #666;
}

#mail-table-container {
  min-height: 500px;
  display: flex;
  flex-direction: column;
}

.logo {
  align-self: center; 
}

#mail-table {
  display: flex;
  flex-direction: column;
  font-family: Arial, Helvetica, sans-serif;
}

#mail-table-header {
  display: flex;
  flex-direction: row;
  background-color: #F2F2F2;
  border-top: 1px solid gray;
  border-bottom: 1px solid gray;
  height: 30px;
  font-weight: bold;
}

#mail-table-header .table-header-title:first-of-type {
  margin-left: 10px;
}

.table-mail-result {
  display: flex;
  flex-direction: row;
  cursor: pointer;
}

.table-mail-result:hover {
  background-color: lightgray;
  opacity: 0.8;
}

.table-mail-result .table-mail-result-data:first-of-type {
  margin-left: 10px;
}

.table-header-title {
  width: 25%;
  display: flex;
  flex-direction: row;
  align-items: center;
  font-size: 13px;
}

.table-mail-result-data {
  height: 30px;
  display: flex;
  flex-direction: row;
  align-items: center;
  position: relative;
}

.mail-result-container {
  display: flex;
  flex-direction: column;
}

.mail-message {
  padding: 5px 10px 10px 10px;
}

.mail-divider {
  border-bottom: 1px solid #666;
}

.mail-more-recipients {
  background-color: gray; 
  color: white;
  border-radius: 5px;
  padding: 2px;
  font-size: 14px;
  position: absolute; 
  right: 50px;
}

.clip {
  width: 15px;
  position: absolute;
  transform: translateX(-30px);
}

.mail-icon-mobile {
  display: none;
}

@media only screen and (max-width: 1200px) {
  .mail-more-recipients,
  .clip {
    display: none;
  }
}

@media only screen and (max-width: 700px) {
  .table-mail-result {
    flex-direction: column;
    position: relative;
  }

  .clip,
  .mail-more-recipients {
    display: block;
  }

  .table-mail-result-data {
    margin-left: 30px !important;
    width: 100% !important;
  }

  .table-mail-result .table-mail-result-data:last-of-type {
    position: absolute;
    right: 10px;
    width: 15% !important;
  }

  .table-mail-result .table-mail-result-data:nth-of-type(3) {
    margin-left: 10px !important;
  }

  .mail-icon-mobile {
    width: 17px;
    display: block;
    position: absolute;
    transform: translate(-20px, 15px);
  }
}
</style>