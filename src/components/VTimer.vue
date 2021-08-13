<template>
    <div class="c-timer">
        <div class="c-timer__counter">
            <span class="c-timer__counter-item">{{ days }}</span> {{ declension }}
            <span class="c-timer__counter-item">{{ hours }}</span> :
            <span class="c-timer__counter-item">{{ minutes }}</span> :
            <span class="c-timer__counter-item">{{ seconds }}</span>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            endtime: {
                required: true,
                type: String // 10 Oct 2021 14:00:00
            }
        },
        beforeMount() {
            this.init()
        },
        data: () => ({
            days: null,
            hours: null,
            minutes: null,
            seconds: null
        }),
        computed: {
            declension() {
                let declension_value = [];
                
                document.documentElement.lang === 'ru'
                ? declension_value = ['день', 'дня', 'дней']
                : declension_value = ['день', 'дні', 'днів'];

                const cases = [2, 0, 1, 1, 1, 2];
                return declension_value[
                    (this.$data.days % 100 > 4
                    && this.$data.days % 100 < 20)
                        ? 2
                        : cases[
                            (this.$data.days % 10 < 5)
                            ? this.$data.days % 10
                            : 5
                        ]
                    ];
            }

        },
        methods: {
            remindTime(endtime) {
                if (typeof endtime !== 'object') {
                    throw new Error('remindTime expects a Date object');
                }

                const total = Date.parse(endtime) - Date.parse(new Date());

                return {
                    total: total,
                    days: Math.floor(total / (1000 * 60 * 60 * 24)),
                    hours: Math.floor((total / (1000 * 60 * 60)) % 24),
                    minutes: Math.floor((total / 1000 / 60) % 60),
                    seconds: Math.floor((total / 1000) % 60)
                };
            },
            init() {
                let timeInterval;
                const endtime = new Date(Date.parse(this.$props.endtime));
                
                const updateTimer = () => {
                    const t = this.remindTime(endtime);

                    if (t.total >= 0) {
                        this.$data.days = (`0${t.days}`).slice(-2);
                        this.$data.hours = (`0${t.hours}`).slice(-2);
                        this.$data.minutes = (`0${t.minutes}`).slice(-2);
                        this.$data.seconds = (`0${t.seconds}`).slice(-2);
                    }

                    if (t.total <= 0) clearInterval(timeInterval);
                }

                updateTimer();
                timeInterval = setInterval(updateTimer, 1000);
            }
        }
    }
</script>