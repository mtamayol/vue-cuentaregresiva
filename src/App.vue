<template>
    <div id="app">
        <h1>Cuenta Regresiva</h1>
        {{ display }}

        <br />
        <div style="padding: 15px 0px 15px 0px;">
            <button v-for="(boton,index) in botones" @click="empiezaCuenta(boton.seg)">
                {{ boton.display }}
            </button>
        </div>

    </div>
</template>
<script>
        export default {
            data() {
                return {
                        now: luxon.DateTime.local(),
                        end: luxon.DateTime.local().plus({ seconds: 3 }),
                        tick: null,
                        botones: [
                          {
                            seg: 3,
                            display: '3s'
                          },  
                          {
                            seg: 60,
                            display: '1m'
                          },
                          {
                            seg: 300,
                            display: '5m'
                          },
                          {
                            seg: 600,
                            display: ' 10m'
                          },
                          {
                            seg: 1800,
                            display: ' 30m'
                          }
                        ]
                };
        },

        methods: {
            empiezaCuenta(seg) {
       
                this.end = luxon.DateTime.local().plus({ seconds: seg+1 });
                this.tick = setInterval(() => {
                  this.now = luxon.DateTime.local()
                }, 100) 
             
            }
        },

              watch: {
                now() {
                  if (this.finished) {
                    clearInterval(this.tick)
                  }
                }
              },
              computed: {
                remaining() {
                  return this.end.diff(this.now).toObject()
                },
                display() {
                   return luxon.Duration.fromObject(this.remaining).toFormat('hh:mm:ss')
                },
                finished() {
                  return this.now >= this.end.minus({ seconds: 1 })
                }
              }
    };
</script>
<style>
    #app {
        font-size: 25px;
        font-family: 'Calibri', sans-serif;
        color: darkblue;
        background: lightblue;
    }
</style>