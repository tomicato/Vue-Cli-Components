<template>
  <div class="wrapper">
    <div class="sample">
      <form @submit.prevent="formSubmited = true" v-if="!formSubmited">
        <div class="progress">
          <div class="progress-bar" :style="progressWidth"></div> <!--:style="progressWidth"-->
        </div>
        <div>
          <app-input v-for="(item, index) in info"
                     :key="index"
                     :name="item.name"
                     :pattern="item.pattern"
                     :value="item.value"
                     @onchange="onChangeData(index, $event)"
          >

          </app-input>
        </div>
        <button class="btn btn-outline-primary" :disabled="done < info.length">
          Send Data
        </button>
      </form>
      <div v-else>
        <table class="table table-bordered">
          <tr v-for="(item, index) in  info">
            <td>{{ item.name }}</td>
            <td>{{ item.value }}</td>
          </tr>
        </table>
      </div>


    </div>
  </div>
</template>

<script>
    import AppInput from './components/Input';

    export default {
        data() {
            return {
                info: [
                    {
                        name: 'Name',
                        value: '',
                        pattern: /^[a-zA-Z ]{2,30}$/
                    },
                    {
                        name: 'Phone',
                        value: '',
                        pattern: /^[0-9]{7,14}$/
                    },
                    {
                        name: 'Email',
                        value: '',
                        pattern: /.+/
                    },
                    {
                        name: 'Some Field 1',
                        value: '',
                        pattern: /.+/
                    },
                    {
                        name: 'Some Field 2',
                        value: '',
                        pattern: /.+/
                    }
                ],
                controls: [],
                done: 0,
                formSubmited: false
            }
        },
        created() {
            for (let i = 0; i < this.info.length; i++) {
               // this.controls.push(false);
                let bool = this.info[i].pattern.test(this.info[i].value);
               this.controls.push(bool);

            }
        },
        methods: {
            onChangeData(index, data) {
                //console.log(data);
                this.info[index].value = data.value;
                this.controls[index] = data.valid;

                let done = 0;
                for (let i = 0; i < this.controls.length; i++) {
                    this.controls[i] ? done++ : '';
                }
                this.done = done;
            }
        },
        computed: {
            progressWidth() {
                return {
                    width: (this.done / this.info.length * 100) + '%'
                }
            }
        },
        components: {
            AppInput
        }
    }
</script>

<style scoped>
  .wrapper {
    max-width: 600px;
    margin: 20px auto;
  }
</style>
