<template>
  <q-page class="q-pa-md">
    <div class="row">
      <div class="col">
        <q-input
          color="purple-12"
          v-model="employee_no"
          label="Employee Number"
        >
          <template v-slot:prepend>
            <q-icon name="person" />
          </template>
        </q-input>
      </div>
      <div class="col">
        <q-select
          filled
          v-model="action"
          :options="actions"
          :option-value="
            (opt) => (Object(opt) === opt && 'id' in opt ? opt.id : null)
          "
          :option-label="
            (opt) =>
              Object(opt) === opt && 'desc' in opt
                ? opt.desc
                : '- Please select action -'
          "
          :option-disable="
            (opt) => (Object(opt) === opt ? opt.inactive === true : true)
          "
          emit-value
          map-options
          style="min-width: 100%; max-width: 100%"
        />
      </div>
      <div class="col">
        <q-btn color="primary" @click="sendLog()">
          <q-icon left size="3em" name="schedule_send" />
          <div>PROCESS</div>
        </q-btn>
      </div>
    </div>

    <div class="row">
      <div class="col">
        <camera ref="camera" />
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import { api } from "boot/axios";
import { useQuasar } from "quasar";
import Camera from "components/Camera.vue";

export default defineComponent({
  name: "PageIndex",
  components: {
    Camera,
  },
  data() {
    return {
      employee_no: "AKB-2022-001",
      action: "am-in",
      actions: [
        {
          id: "am-in",
          desc: "AM IN",
        },
        {
          id: "am-out",
          desc: "AM OUT",
        },
        {
          id: "pm-in",
          desc: "PM IN",
        },
        {
          id: "pm-out",
          desc: "PM OUT",
        },
        {
          id: "ot-in",
          desc: "OT IN",
        },
        {
          id: "ot-in",
          desc: "OT IN",
        },
      ],
    };
  },
  mounted() {
    // navigator.getUserMedia(
    //   {
    //     video: {},
    //   },
    //   (stream) => (this.$refs.camera.srcObject = stream),
    //   (err) => console.error(err)
    // );
  },
  methods: {
    openCamera() {
      this.$refs.camera;
    },
    sendLog() {
      if (this.employee_no === "" || this.action === "") {
        this.$q.notify({
          message: "Please provide employee number and action",
          icon: "warning",
          color: "red",
        });
      } else {
        api
          .post("/api/capture-log/" + this.employee_no + "/" + this.action, {})
          .then((res) => {
            var message = JSON.parse(res.request.response).message;
            this.$q.notify({
              message: message,
              icon: "check_circle",
              color: "green",
            });
          })
          .catch((err) => {
            var message = JSON.parse(err.request.response).message;
            this.$q.notify({
              message: message,
              icon: "warning",
              color: "red",
            });
          });
      }
    },
  },
});
</script>
