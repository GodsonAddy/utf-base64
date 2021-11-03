<template>
  <div>
    <b-container fluid>
      <b-row class="justify-content-md-center">
        <b-col cols="6">
          <b-form @submit="onSubmit">
            <b-form-group id="msisdn" label="Phone Number:" label-for="msisdn">
              <b-form-input
                id="msisdn"
                v-model="tobi.phoneNumber"
                type="text"
                placeholder="Enter your phone number"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group id="otp" label="OTP" label-for="otp">
              <b-form-input
                id="otp"
                v-model="tobi.otpNumber"
                placeholder="Your otp number"
                type="text"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group id="name" label="Name:" label-for="name">
              <b-form-input
                id="name"
                v-model="tobi.name"
                type="text"
                placeholder="Your name"
                required
              ></b-form-input>
            </b-form-group>

            <b-button
              type="submit"
              variant="success"
              v-if="!loading"
              block
              :disabled="validated"
            >
              send
            </b-button>
            <b-button
              type="submit"
              variant="success"
              v-if="loading"
              disabled
              block
            >
              <b-spinner small type="grow"></b-spinner>
              Loading...
            </b-button>
          </b-form>

          <h5 v-bind:id="utfinfo">
            {{ utfinfo }}
          </h5>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Swal from "sweetalert2";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      tobi: {
        phoneNumber: "",
        otpNumber: "",
        name: "",
      },
      loading: false,
      utfinfo: "",
    };
  },
  computed: {
    validated() {
      return !this.tobi.phoneNumber || !this.tobi.otpNumber || !this.tobi.name;
    },
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      try {
        const msisdn = window.btoa(this.tobi.phoneNumber);
        const otp = window.btoa(this.tobi.otpNumber);
        const TOBi = window.btoa(this.tobi.name);
        const response = "Encoded String: " + msisdn + otp + TOBi;
        this.utfinfo = msisdn + ":" + otp + ":" + TOBi + "\n" + response;
        this.tobi.name = "";
        this.tobi.phoneNumber = "";
        this.tobi.otpNumber = "";
        Swal.fire(
          "Success",
          "Your credentials was successfully sent",
          "success"
        );
      } catch (error) {
        console.log(error);
        Swal.fire("Error", "Invalid Credentials. Try again!", "error");
      }
    },
  },
};
</script>

<style scoped></style>
