<template>
  <div id="mail-container">
    <p id="result-text">Results: {{FilteredResults.length}} mail(s)</p>
    <hr v-if="!results.length" class="divider" style="transform: translateY(-10px)">

    <div id="mail-table-container" :style="{justifyContent: results.length ? 'flex-start' : 'center'}">
      <template v-if="!results.length">
        <img src="../assets/png/logo.png" height="100" width="100" class="logo">
      </template>

      <template v-else>
        <div id="mail-table">
          <div id="mail-table-header">
            <div class="table-header-title" @click="TitleClicked('From')">
              <div :class="[sort == 'From' ? 'black' : 'normal']">From</div>
              <ArrowUp 
                v-if="sort == 'From'" 
                class="arrow-up" 
                :style="{transform: sortByAsc ? null : 'rotate(180deg)'}" />
            </div>
            <div class="table-header-title" @click="TitleClicked('To')">
              <div :class="[sort == 'To' ? 'black' : 'normal']">To</div>
              <ArrowUp 
                v-if="sort == 'To'" 
                class="arrow-up" 
                :style="{transform: sortByAsc ? null : 'rotate(180deg)'}" />
            </div>
            <div class="table-header-title" style="width: 35%;" @click="TitleClicked('Subject')">
              <div :class="[sort == 'Subject' ? 'black' : 'normal']">Subject</div>
              <ArrowUp 
                v-if="sort == 'Subject'" 
                class="arrow-up" 
                :style="{transform: sortByAsc ? null : 'rotate(180deg)'}" />
            </div>
            <div class="table-header-title" style="width: 15%;" @click="TitleClicked('Date')"> 
              <div :class="[sort == 'Date' ? 'black' : 'normal']">Date</div>
              <ArrowUp 
                v-if="sort == 'Date'" 
                class="arrow-up" 
                :style="{transform: sortByAsc ? null : 'rotate(180deg)'}" />
            </div>
          </div>
          <div class="mail-result-container" v-for="(r,i) in FilteredResults" :key="i">
            <div class="table-mail-result" @click="r.showMail = !r.showMail">
              <div class="table-mail-result-data" style="width: 25%;">
                <MailIcon class="mail-icon-mobile" />
                <div :class="['table-mail-sender', sort == 'From' ? 'bold' : null]">{{r.from}}</div>
              </div>

              <div class="table-mail-result-data" style="width: 25%;">
                <div :class="['table-mail-recipient', sort == 'To' ? 'bold' : null]">
                  {{r.recipients.length == 1 ? r.to : `${r.recipients[0]}, ...`}}
                </div>
                <div v-if="r.recipients.length > 1" class="mail-more-recipients">
                  + {{r.recipients.length - 1}}
                </div>
              </div>

              <div :class="['table-mail-result-data', 'overflow', sort == 'Subject' ? 'bold' : null]" style="width: 35%;">
                {{r.subject}}
              </div>

              <div :class="['table-mail-result-data', sort == 'Date' ? 'bold' : null]" style="width: 15%;">
                <Clip v-if="r.attachments" class="clip" />
                {{LegibleDateFormat(r.date)}}
                <ArrowRight class="arrow-right" />
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
import ArrowUp from "@/assets/svg/icon_arrow01.svg"
import ArrowRight from "@/assets/svg/icon_arrow02.svg"
import { EventBus } from "@/services/event-bus.js"

export default {
  name: "Mail",
  components: {
    Clip,
    MailIcon,
    ArrowUp,
    ArrowRight,
  },
  data() {
    return {
      results: [],
      startDate: {},
      endDate: {},
      months: ["Jan", "Feb", "March", "April", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
      sort: 'From',
      sortByAsc: null
    }
  },
  methods: {
    GetAllResults() {
      return {
        status: "200",
        data: [
          {
            from: 'from1@example.com',
            to: 'to4@example.com',
            subject: '[ HR-888 ] Notice of official accouncement',
            date: "6/2/2020 4:44",
            message: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris sodales interdum maximus. Vestibulum nec nunc efficitur, laoreet eros vel, pellentesque odio. Cras at diam molestie, dictum orci sit amet, egestas nulla. Donec sollicitudin diam ac libero luctus volutpat. In auctor sit amet libero id dictum. Suspendisse potenti. Etiam tincidunt odio ac justo lacinia faucibus. Donec tincidunt maximus leo, sit amet cursus felis gravida vitae. Cras et risus sed nulla lacinia egestas. Aliquam venenatis odio a est fermentum faucibus.",
            attachments: 0
          },
          {
            from: 'from2@example.com',
            to: 'to3@example.com,to@example.com,to@example.com',
            subject: '[ HR-888 ] Notice of official accouncement',
            date: "6/3/2020 4:44",
            message: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris sodales interdum maximus. Vestibulum nec nunc efficitur, laoreet eros vel, pellentesque odio. Cras at diam molestie, dictum orci sit amet, egestas nulla. Donec sollicitudin diam ac libero luctus volutpat. In auctor sit amet libero id dictum. Suspendisse potenti. Etiam tincidunt odio ac justo lacinia faucibus. Donec tincidunt maximus leo, sit amet cursus felis gravida vitae. Cras et risus sed nulla lacinia egestas. Aliquam venenatis odio a est fermentum faucibus",
            attachments: 0
          },
          {
            from: 'from3@example.com',
            to: 'to2@example.com,to@example.com',
            subject: '[ HR-888 ] Notice of official accouncement',
            date: "6/4/2020 4:44",
            message: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris sodales interdum maximus. Vestibulum nec nunc efficitur, laoreet eros vel, pellentesque odio. Cras at diam molestie, dictum orci sit amet, egestas nulla. Donec sollicitudin diam ac libero luctus volutpat. In auctor sit amet libero id dictum. Suspendisse potenti. Etiam tincidunt odio ac justo lacinia faucibus. Donec tincidunt maximus leo, sit amet cursus felis gravida vitae. Cras et risus sed nulla lacinia egestas. Aliquam venenatis odio a est fermentum faucibus",
            attachments: 1
          },
          {
            from: 'from4@example.com',
            to: 'to1@example.com',
            subject: '[ HR-888 ] Notice of official accouncement',
            date: "7/18/2020 4:44",
            message: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris sodales interdum maximus. Vestibulum nec nunc efficitur, laoreet eros vel, pellentesque odio. Cras at diam molestie, dictum orci sit amet, egestas nulla. Donec sollicitudin diam ac libero luctus volutpat. In auctor sit amet libero id dictum. Suspendisse potenti. Etiam tincidunt odio ac justo lacinia faucibus. Donec tincidunt maximus leo, sit amet cursus felis gravida vitae. Cras et risus sed nulla lacinia egestas. Aliquam venenatis odio a est fermentum faucibus",
            attachments: 2
          }
        ]
      }
    },
    IsToday(argDate) {
      const today = new Date()
      
      if (today.getFullYear() == argDate.getFullYear() &&
          today.getMonth() ==  argDate.getMonth() &&
          today.getDate() == argDate.getDate()) {
        return true
      }
      
      return false
    },
    LegibleDateFormat(date) {
      const dateObj = new Date(date)

      if (this.IsToday(dateObj)) {
        return date.split(' ')[1]
      }

      return `${this.months[dateObj.getMonth()]} ${dateObj.getDate()}`
    },
    SortByTitle(x, y) {
      if (this.sort == 'From') {
        return this.CompareFn(x.from, y.from)
      } else if (this.sort == 'To') {
        return this.CompareFn(x.to, y.to)
      } else if (this.sort == 'Subject') {
        return this.CompareFn(x.subject, y.subject)
      } else {
        return this.CompareFn(new Date(x.date), new Date(y.date))
      }
    },
    CompareFn(a, b) {
      let val = 0

      if (a > b) {
        val = 1;
      }
      
      if (a < b) {
        val = -1;
      }

      if (this.sortByAsc) {
        val = val * -1;
      }

      return val
    },
    TitleClicked(arg) {
      if (this.sort == arg) {
        this.sortByAsc = !this.sortByAsc;
      }

      this.sort = arg;
    }
  },
  created() {
    // mocking server request & processing 
    setTimeout(() => {
      const response = this.GetAllResults();

      if (response.status == 200) {
        this.results = response.data.map(r => ({...r, showMail: false, recipients: r.to.split(',')}));
      }
    }, 1000)
    
    // register datesChanged event listener
    EventBus.$on('datesChanged', (dates) => {
      this.startDate = dates['start']
      this.endDate = dates['end']
    });
  },
  computed: {
    FilteredResults() {
      return this.results
        .filter(r => {
          const dateObj = new Date(r.date)
          return dateObj >= this.startDate && dateObj <= this.endDate
        })
        .sort((a, b) => this.SortByTitle(a, b))
    }
  },
  destroyed() {
    EventBus.$off('datesChanged');
  }
}
</script>

<style>
#mail-container {
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

.table-header-title:first-of-type {
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

.table-mail-result-data:first-of-type {
  margin-left: 10px;
}

.table-header-title {
  width: 25%;
  display: flex;
  flex-direction: row;
  align-items: center;
  font-size: 13px;
  cursor: pointer;
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

.arrow-right {
  display: none;
  width: 4px;
  margin-left: 5px;
}

.overflow {
  white-space: nowrap;
  overflow: hidden;
}

.arrow-up {
  width: 8px;
  margin-left: 5px;
}

.black {
  color: black;
}

.normal {
  color: gray;
}

.bold {
  font-weight: bold;
}

@media only screen and (max-width: 1200px) {
  .mail-more-recipients,
  .clip {
    display: none;
  }
}

@media only screen and (max-width: 700px) {
  .table-header-title {
    width: auto !important;
    padding-left: 5px;
    padding-right: 5px;
    position: relative;
  }

  .table-header-title:nth-child(n+2):after {
    content: '';
    position: absolute;
    left: 0px;
    top: 35%;
    height: 35%;
    border-left: 1px solid black;
  }

  .table-mail-result {
    flex-direction: column;
    position: relative;
  }

  .arrow-right,
  .clip,
  .mail-more-recipients {
    display: block;
  }

  .table-mail-result-data {
    margin-left: 30px !important;
    width: 100% !important;
  }

  .table-mail-sender, 
  .table-mail-recipient {
    margin-left: 5px;
  }

  .table-mail-result-data:last-of-type {
    position: absolute;
    right: 10px;
    width: auto !important;
  }

  .table-mail-result-data:nth-of-type(3) {
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