<template>
  <div>
    <div class="card-task">
      <div class="card-task-content">
        <div class="title">
          <h2>Задача № {{ task.number_task }}</h2>
        </div>
        <div
            v-if="task.description_task"
            class="description_task task-text">
          <span>{{ task.description_task }}</span>
        </div>
        <div
            v-if="task.time_start && task.category_task !== 'plan'"
            class="time_start task-text">
          <h2>Дата и время начала</h2><span>{{ task.time_start.toLocaleString() }}</span>
        </div>
        <div
            v-if="task.time_total"
            class="time_total task-text">
          <h2>Ушло времени</h2>
          <span>
          {{ Math.trunc(task.time_total/(3600000*24)) ? Math.trunc(task.time_total/(3600000*24)) + " дня": null }}
          {{ Math.trunc(task.time_total/3600000) ? Math.trunc(task.time_total/3600000)%24 + " часов": null }}
          {{ Math.trunc(task.time_total/60000) ? Math.trunc(task.time_total/60000)%60 + " минут": null }}
          {{ Math.trunc(task.time_total/1000) ? Math.trunc(task.time_total/1000)%60 + " секунд": null }}
          </span>
        </div>
        <div
            v-if="task.name_employee"
            class="name_employee task-text">
          <h2>Ответсвенный</h2><span>{{ task.name_employee }}</span>
        </div>
      </div>

      <div class="block-button" v-if="task.category_task === 'ready'">
        <button
            v-on:click="$emit('edit-task', task)"
            class="edit-card-button card-button">
          <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px"
               width="40" height="40"
               viewBox="0 0 226 226"
               style=" fill:black;">
            <g fill="none" fill-rule="nonzero" stroke="none" stroke-width="1" stroke-linecap="butt"
               stroke-linejoin="miter" stroke-miterlimit="10" stroke-dasharray="" stroke-dashoffset="0"
               font-family="none" font-size="none" style="mix-blend-mode: normal">
              <path d="M0,226v-226h226v226z" fill="none"></path>
              <g fill="black">
                <path
                    d="M200.43375,9.04c-4.25516,0 -8.63391,1.57141 -11.865,4.8025l-6.07375,6.215l23.4475,23.4475c-0.01766,0.01766 6.215,-6.07375 6.215,-6.07375c6.47984,-6.47984 6.47984,-17.10891 0,-23.58875c-3.24875,-3.24875 -7.46859,-4.8025 -11.72375,-4.8025zM175.15,26.8375l-102.68875,102.68875l-0.2825,1.4125l-4.2375,21.89375l-1.4125,6.63875l6.63875,-1.4125l21.89375,-4.2375l1.4125,-0.2825l102.68875,-102.68875l-6.4975,-6.35625l-100.57,100.42875l-11.0175,-11.0175l100.42875,-100.57zM13.56,45.2c-2.50719,0 -4.52,2.03047 -4.52,4.52v162.72c0,2.48953 2.01281,4.52 4.52,4.52h162.72c2.50719,0 4.52,-2.03047 4.52,-4.52v-131.08l-9.04,9.04v117.52h-153.68v-153.68h117.52l9.04,-9.04z"></path>
              </g>
            </g>
          </svg>
        </button>
        <button
            v-on:click="$emit('change-status', task.number_task)"
            class="edit-status-card-button card-button">
          <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px"
               width="40" height="40"
               viewBox="0 0 172 172"
               style=" fill:black;">
            <g fill="none" fill-rule="nonzero" stroke="none" stroke-width="1" stroke-linecap="butt"
               stroke-linejoin="miter" stroke-miterlimit="10" stroke-dasharray="" stroke-dashoffset="0"
               font-family="none" font-size="none"
               style="mix-blend-mode: normal">
              <path d="M0,172v-172h172v172z" fill="none"></path>
              <g fill="black">
                <path
                    d="M86,6.88c-43.65844,0 -79.12,35.46156 -79.12,79.12c0,43.65844 35.46156,79.12 79.12,79.12c43.65844,0 79.12,-35.46156 79.12,-79.12c0,-43.65844 -35.46156,-79.12 -79.12,-79.12zM86,13.76c39.93625,0 72.24,32.30375 72.24,72.24c0,39.93625 -32.30375,72.24 -72.24,72.24c-39.93625,0 -72.24,-32.30375 -72.24,-72.24c0,-39.93625 32.30375,-72.24 72.24,-72.24zM59.555,51.4925l22.79,33.755l-23.005,34.185h10.32l17.63,-26.23h0.7525l17.415,26.23h10.965l-22.8975,-33.8625l23.435,-34.0775h-10.535l-17.63,26.5525h-0.86l-17.2,-26.5525z"></path>
              </g>
            </g>
          </svg>
        </button>
      </div>

      <div class="block-button" v-if="task.category_task === 'work' || task.category_task === 'plan'">
        <button
            v-on:click="$emit('edit-task', task)"
            class="edit-card-button card-button">
          <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px"
               width="40" height="40"
               viewBox="0 0 226 226"
               style=" fill:black;">
            <g fill="none" fill-rule="nonzero" stroke="none" stroke-width="1" stroke-linecap="butt"
               stroke-linejoin="miter" stroke-miterlimit="10" stroke-dasharray="" stroke-dashoffset="0"
               font-family="none" font-size="none" style="mix-blend-mode: normal">
              <path d="M0,226v-226h226v226z" fill="none"></path>
              <g fill="black">
                <path
                    d="M200.43375,9.04c-4.25516,0 -8.63391,1.57141 -11.865,4.8025l-6.07375,6.215l23.4475,23.4475c-0.01766,0.01766 6.215,-6.07375 6.215,-6.07375c6.47984,-6.47984 6.47984,-17.10891 0,-23.58875c-3.24875,-3.24875 -7.46859,-4.8025 -11.72375,-4.8025zM175.15,26.8375l-102.68875,102.68875l-0.2825,1.4125l-4.2375,21.89375l-1.4125,6.63875l6.63875,-1.4125l21.89375,-4.2375l1.4125,-0.2825l102.68875,-102.68875l-6.4975,-6.35625l-100.57,100.42875l-11.0175,-11.0175l100.42875,-100.57zM13.56,45.2c-2.50719,0 -4.52,2.03047 -4.52,4.52v162.72c0,2.48953 2.01281,4.52 4.52,4.52h162.72c2.50719,0 4.52,-2.03047 4.52,-4.52v-131.08l-9.04,9.04v117.52h-153.68v-153.68h117.52l9.04,-9.04z"></path>
              </g>
            </g>
          </svg>
        </button>
        <button
            v-on:click="$emit('change-status', task.number_task)"
            class="edit-status-card-button card-button">
          <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px"
               width="40" height="40"
               viewBox="0 0 172 172"
               style=" fill:black;">
            <g fill="none" fill-rule="nonzero" stroke="none" stroke-width="1" stroke-linecap="butt"
               stroke-linejoin="miter" stroke-miterlimit="10" stroke-dasharray="" stroke-dashoffset="0"
               font-family="none" font-size="none" style="mix-blend-mode: normal">
              <path d="M0,172v-172h172v172z" fill="none"></path>
              <g>
                <path
                    d="M169.34609,86c0,-46.02344 -37.32266,-83.34609 -83.34609,-83.34609c-46.02344,0 -83.34609,37.32266 -83.34609,83.34609c0,46.02344 37.32266,83.34609 83.34609,83.34609c46.02344,0 83.34609,-37.32266 83.34609,-83.34609z"
                    fill="black"></path>
                <path
                    d="M131.88906,58.08359c-1.94844,-5.07266 -5.94609,-4.26641 -10.27969,-3.39297c-2.58672,0.5375 -14.10937,3.89687 -32.28359,23.1125c-7.55859,7.96172 -12.53047,14.31094 -15.82266,19.14844c-2.01562,-2.45234 -4.3,-5.10625 -6.71875,-7.49141c-7.42422,-7.42422 -15.72187,-12.53047 -16.05781,-12.73203c-3.46016,-2.11641 -7.99531,-1.04141 -10.14531,2.45234c-2.11641,3.46016 -1.04141,7.99531 2.41875,10.14531c0.06719,0.03359 7.18906,4.43438 13.30313,10.58203c6.24844,6.24844 11.92578,14.71406 11.99297,14.81484c1.37734,2.08281 3.69531,3.29219 6.14766,3.29219c0.40312,0 0.83984,-0.03359 1.27656,-0.10078c2.88906,-0.50391 5.17344,-2.65391 5.87891,-5.47578c0.03359,-0.06719 2.95625,-8.16328 18.37578,-24.42266c12.42969,-13.13516 20.72734,-17.30078 23.61641,-18.44297c0.03359,0 0.03359,0 0.10078,0c0,0 0.10078,-0.03359 0.26875,-0.13438c0.50391,-0.20156 0.77266,-0.26875 0.77266,-0.26875c-0.13437,0.03359 -0.20156,0.03359 -0.20156,0.03359v-0.03359c1.34375,-0.57109 3.82969,-1.64609 3.86328,-1.67969c3.72891,-1.6125 4.97187,-5.64375 3.49375,-9.40625z"
                    fill="#ffffff"></path>
              </g>
            </g>
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: "CardTask",
  props: ["task"]
}
</script>

<style>

.card-task {
  display: grid;
  width: 100%;
  height: 330px;
  background-color: var(--color-background-blue);
  padding: 10px;
  margin: 15px -10px;
  grid-template-rows: auto 50px;
}

.card-task-content {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: repeat(5, 1fr);
}


.title h2 {
  color: var(--color-base-blue);
  font-size: 30px;
}

.task-text h2 {
  color: var(--color-base-blue);
  font-weight: bold;
  font-size: 18px;
}

.task-text span {
  color: var(--color-life-dark);
  font-size: 16px;
}

.block-button {
  display: flex;
  justify-self: end;
  margin: 0 0 -31px 0;
}

.card-button {
  background: none;
  border: none;
  cursor: pointer;
  max-height: 50px;
}

.card-button:active, .card-button:focus {
  outline: none;
}

</style>
