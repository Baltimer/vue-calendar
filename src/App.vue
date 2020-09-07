<template>
  <div id="app" class="h-screen flex-col justify-center pt-5 pb-3 bg-gray-300">
    <div id="calendar" class="w-full sm:w-2/3 md:w-3/5 lg:w-1/3 rounded border px-2 sm:px-0">
      <section class="flex justify-evenly px-1 mb-1">
        <a v-on:click="changeMonth(-1)" class="flex-shrink cursor-pointer text-gray-800 underline">{{'< ' + previousMonth}}</a>
        <h2 class="flex-grow text-center text-lg font-bold">{{month}} {{year}}</h2>
        <a v-on:click="changeMonth(1)" class="flex-shrink cursor-pointer text-gray-800 underline">{{nextMonth + ' >'}}</a>
      </section>
      <ul class="flex bg-gray-100 border-t text-gray-600">
        <li class="flex-1 text-center border-r last:border-r-0">L</li>
        <li class="flex-1 text-center border-r last:border-r-0">M</li>
        <li class="flex-1 text-center border-r last:border-r-0">M</li>
        <li class="flex-1 text-center border-r last:border-r-0">J</li>
        <li class="flex-1 text-center border-r last:border-r-0">V</li>
        <li class="flex-1 text-center border-r last:border-r-0">S</li>
        <li class="flex-1 text-center border-r last:border-r-0">D</li>
      </ul>
      <section>
        <ul v-for="(week, weekIndex) in weeks" :key="weekIndex" class="border-t">
          <li>
            <ul class="flex">
              <li v-for="(day, dayIndex) in week" :key="`${weekIndex}-${dayIndex}`" class="flex-1 p-1 text-center border-r last:border-r-0" :class="day != null && day.events.length > 0 ? 'modal-open' : ''" @click="day != null && day.events.length ? events = day.events : ''">
                <div class="flex flex-col">
                  <span>{{day && day.date.getDate()}}</span>
                  <div v-if="day != null">
                    <span v-for="(event, index) in day.events" :key="index" class="font-bold" :class="event.color == 'blue' ? 'text-blue-600' : event.color == 'green' ? 'text-green-600' : event.color == 'yellow' ? 'text-yellow-600' : ''">{{'·'}}</span>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </section>
    </div>
    <div id="events" class="w-full flex-1 p-2 overflow-auto">
      <ul class="list-none">
        <li class="w-full rounded border border-blue-600 p-2">Test</li>
        <li class="w-full rounded border border-yellow-600 p-2 mt-2">Test</li>
        <li class="w-full rounded border border-green-600 p-2 mt-2">Test</li>
        <li class="w-full rounded border border-black p-2 mt-2">Test</li>
        <li class="w-full rounded border p-2 mt-2">Test</li>
        <li class="w-full rounded border p-2 mt-2">Test</li>
        <li class="w-full rounded border p-2 mt-2">Test</li>
        <li class="w-full rounded border p-2 mt-2">Test</li>
        <li class="w-full rounded border p-2 mt-2">Test</li>
      </ul>
    </div>

    <!-- MODAL -->
    <div class="modal opacity-0 pointer-events-none fixed w-full h-full top-0 left-0 flex items-center justify-center">
      <div class="modal-overlay absolute w-full h-full bg-gray-900 opacity-50"></div>
      
      <div class="modal-container bg-gray-200 w-11/12 md:max-w-5xl max-h-90 min-h-half mx-auto rounded shadow-lg z-50 overflow-y-auto">
        
        <div class="modal-close absolute top-0 right-0 cursor-pointer flex flex-col items-center mt-4 mr-4 text-white text-sm z-50">
          <svg class="fill-current text-white" xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 18 18">
            <path d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"></path>
          </svg>
          <span class="text-sm">(Esc)</span>
        </div>

        <!-- Add margin if you want to see some of the overlay behind the modal-->
        <div class="modal-content py-4 text-left px-6">
          <!--Title-->
          <div class="flex justify-between items-center pb-3">
            <p class="text-2xl font-bold">Events</p>
            <div class="modal-close cursor-pointer z-50">
              <svg class="fill-current text-black" xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 18 18">
                <path d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"></path>
              </svg>
            </div>
          </div>

          <!--Body-->
          <div>
            <section class="text-gray-700 body-font">
              <div class="container py-5 mx-auto w-full flex flex-col flex-wrap">
                <div v-for="event in events" :key="event.id" class="flex relative pt-1 pb-2 sm:items-center md:w-2/3">
                  <div class="h-full w-6 absolute inset-0 flex items-center justify-center">
                    <div class="h-full w-1 pointer-events-none"
                      :class="event.color == 'blue' ? 'bg-blue-600' : event.color == 'green' ? 'bg-green-600' : event.color == 'yellow' ? 'bg-yellow-600' : 'bg-black'"></div>
                  </div>
                  <div class="flex-shrink-0 w-12 h-6 rounded-full mt-10 sm:mt-0 inline-flex items-center justify-center text-white relative z-10 title-font font-medium text-sm"
                    :class="event.color == 'blue' ? 'bg-blue-600' : event.color == 'green' ? 'bg-green-600' : event.color == 'yellow' ? 'bg-yellow-600' : 'bg-black'">{{event.hour}}</div>
                  <div class="flex-grow md:pl-8 pl-6 flex sm:items-center items-start flex-col sm:flex-row">
                    <div class="flex-grow sm:pl-6 mt-6 sm:mt-0">
                      <h2 class="font-medium title-font text-gray-900 mb-1 text-xl">{{event.name}}</h2>
                      <p class="leading-relaxed">{{event.description}}</p>
                    </div>
                  </div>
                </div>
              </div>
            </section>
          </div>

          <!--Footer-->
          <div class="flex justify-end mt-2 pt-2 border-t-2 border-gray-300">
            <button class="px-4 py-1 rounded-lg border shadow-lg text-indigo-500 focus:outline-none">New</button>
          </div>
          
        </div>
      </div>
    </div>
  </div>
</template>

<script>
var lodashArray = require('lodash/array');
import { format, startOfMonth, endOfMonth, getDaysInMonth, getDay, setDate, addMonths } from 'date-fns'
import { es } from 'date-fns/locale';

const colors = ["blue", "green", "yellow"]

export default {
  name: "app",
  data: () => { 
    return {
      date: new Date(2020, 7, 1),
      events: []
    }
  },
  computed: {
      month: function() {
        return this.capitalize(format(this.date, 'MMMM', {locale: es}))
      },
      year: function() {
        return format(this.date, 'yyyy', {locale: es})
      },
      previousMonth: function() {
        return this.capitalize(format(addMonths(this.date, -1), 'MMMM', {locale: es}))
      },
      nextMonth: function() {
        return this.capitalize(format(addMonths(this.date, +1), 'MMMM', {locale: es}))
      },
      startOfMonth: function() {
        return startOfMonth(this.date)
      },
      endOfMonth: function() {
        return endOfMonth(this.date)
      },
      daysInMonth: function() {
        return getDaysInMonth(this.date)
      },
      weeks: function() {
        var firstDayOfWeek = getDay(this.startOfMonth) - 1

        var days = []

        for (let i = 0; i < firstDayOfWeek; i++) {
          days.push(null)
        }

        for (let i = 1; i <= this.daysInMonth; i++) {
          let dayObject = {
            date: setDate(this.date, i),
            events: this.randomEvents()
          }
          days.push(dayObject)
        }

        var lastDayOfWeek = getDay(this.endOfMonth)
        for (let i = 0; i < 6 - lastDayOfWeek; i++) {
          days.push(null)
        }

        return lodashArray.chunk(days, 7)
      }
    },
    methods: {
      changeMonth: function(by) {
        this.date = addMonths(this.date, by)
      },
      capitalize: (s) => {
        if (typeof s !== 'string') return ''
        return s.charAt(0).toUpperCase() + s.slice(1)
      },
      randomEvents: (minEvents = 0, maxEvents = 3) => {
        let events = [];
        for(let i = 0; i < Math.floor(Math.random() * (maxEvents - minEvents + 1) + minEvents); i++) {
          events.push({
            id: i + 1,
            name: `Event-${i + 1}`,
            hour: `1${i}:00`,
            description: "Lorem ipsum",
            color: colors[Math.floor(Math.random() * (3 - 0 + 1) + 0)]
          })
        }
        return events;
      },
      toggleModal () {
        const body = document.querySelector('body')
        const modal = document.querySelector('.modal')
        modal.classList.toggle('opacity-0')
        modal.classList.toggle('pointer-events-none')
        body.classList.toggle('modal-active')
      }
    },
    mounted() {
      // MODAL SECTION
      var that = this;
      var openmodal = document.querySelectorAll('.modal-open')
      for (var i = 0; i < openmodal.length; i++) {
        openmodal[i].addEventListener('click', function(event){
        event.preventDefault()
        that.toggleModal()
        })
      }
      const overlay = document.querySelector('.modal-overlay')
      overlay.addEventListener('click', that.toggleModal)

      var closemodal = document.querySelectorAll('.modal-close')
      for (var i = 0; i < closemodal.length; i++) {
        closemodal[i].addEventListener('click', that.toggleModal)
      }

      document.onkeydown = function(evt) {
        evt = evt || window.event
        var isEscape = false
        if ("key" in evt) {
        isEscape = (evt.key === "Escape" || evt.key === "Esc")
        } else {
        isEscape = (evt.keyCode === 27)
        }
        if (isEscape && document.body.classList.contains('modal-active')) {
        that.toggleModal()
        }
      };
      // END MODAL SECTION
    },
};
</script>

<style>
    .modal {
      transition: opacity 0.25s ease;
    }
    body.modal-active {
      overflow-x: hidden;
      overflow-y: visible !important;
    }
  </style>