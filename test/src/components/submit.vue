<template>
  <div class="center">
    <b-button @click="submit" :disabled="!valid" type="is-success" rounded>Submit</b-button>
  </div>
</template>

<script>
import format from "date-fns/lightFormat";

export default {
    name: "submit",
    props: {
        amount: Number,
        cardData: Object,
        valid: Boolean,
        doSubmit: Boolean
    },
    methods: {
        submit() {
            let request = {
                xKey: "",
                xSoftwareName: "Test-Vue-iFields",
                xSoftwareVersion: "1.0",
                xVersion: "4.5.8",
                xCommand: "cc:sale",
                xAmount: this.amount,
                xCardnum: this.cardData.cardToken,
                xExp: format(
                    new Date(this.cardData.year, this.cardData.month - 1),
                    "MMyy"
                ),
                xCvv: this.cardData.cvvToken
            };
            console.log(request);
            fetch(
                "https://x1.cardknox.com/gateway?" +
                    Object.entries(request)
                        .map(e => e.map(e1 => encodeURIComponent(e1)).join("="))
                        .join("&")
            )
                .then(r => r.json())
                .then(console.log)
                .catch(console.error);
        }
    },
    watch: {
        doSubmit(val){
            if(val)
                this.submit();
        }
    }
};
</script>