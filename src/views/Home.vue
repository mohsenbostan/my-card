<template>
    <div class="home">
        <vs-row vs-justify="center">
            <vs-button v-if="editMode === false" class="vs-divider" style="margin: 0 1rem;" @click="addMode = !addMode">
                {{ !addMode ? 'اضافه کردن کارت جدید' : 'بازگشت' }}
            </vs-button>
            <vs-button v-if="editMode === true" class="vs-divider" style="margin: 0 1rem;"
                       @click="editMode = !editMode">
                بازگشت
            </vs-button>
            <vs-divider color="warning" icon="star"></vs-divider>

            <!-- Add New Card -->
            <vs-col v-if="addMode === true && editMode !== true " type="flex" vs-justify="center" vs-align="center"
                    vs-w="12"
                    style="padding: 1em 2em">
                <vs-card>
                    <div slot="header">
                        <h3>
                            افزودن کارت جدید
                        </h3>
                    </div>
                    <div>
                        <vs-alert title="خطا" v-if="hasError === true" color="danger">
                            تمام موارد را صحیح وارد کنید.
                        </vs-alert>
                        <div style="font-size: 32px; padding-bottom: 1rem;">
                            <vs-input class="inputx" style="width: 100%" size="large" label="نام کارت"
                                      placeholder="نام کارت"
                                      v-model="newCard.name"/>
                        </div>
                        <div style="font-size: 32px; padding-bottom: 1rem;">
                            <vs-input style="width: 100%" size="large" class="inputx" label="شماره کارت"
                                      placeholder="شماره کارت"
                                      v-model="newCard.number"/>
                        </div>
                        <div style="font-size: 32px; padding-bottom: 1rem;">
                            <vs-input style="width: 100%" size="large" class="inputx" label="CVV2"
                                      placeholder="CVV2"
                                      v-model="newCard.cvv"/>
                        </div>
                        <div style="font-size: 32px;">
                            <vs-input style="width: 100%" class="inputx" size="large" label="تاریخ انقضا"
                                      placeholder="تاریخ انقضا"
                                      v-model="newCard.date"/>
                        </div>
                        <div style="font-size: 32px;">
                            <label for="logo">لوگو بانک</label>
                            <br>
                            <input type="file" @change="logoSelected" id="logo" required>
                        </div>
                    </div>
                    <div slot="footer">
                        <vs-row vs-justify="flex-end">
                            <vs-button color="success" @click="saveCard()" icon="save" class="vs-divider">ذخیره
                            </vs-button>
                        </vs-row>
                    </div>
                </vs-card>
            </vs-col>

            <!-- Edit Card -->
            <vs-col v-if="editMode === true && addMode !== true" type="flex" vs-justify="center" vs-align="center"
                    vs-w="12"
                    style="padding: 1em 2em">
                <vs-card>
                    <div slot="header">
                        <h3>
                            ویرایش کارت
                        </h3>
                    </div>
                    <div>
                        <vs-alert title="خطا" v-if="hasError === true" color="danger">
                            تمام موارد را صحیح وارد کنید.
                        </vs-alert>
                        <div style="font-size: 32px; padding-bottom: 1rem;">
                            <vs-input class="inputx" style="width: 100%" size="large" label="نام کارت"
                                      placeholder="نام کارت"
                                      v-model="editCard.name"/>
                        </div>
                        <div style="font-size: 32px; padding-bottom: 1rem;">
                            <vs-input style="width: 100%" size="large" class="inputx" label="شماره کارت"
                                      placeholder="شماره کارت"
                                      v-model="editCard.number"/>
                        </div>
                        <div style="font-size: 32px; padding-bottom: 1rem;">
                            <vs-input style="width: 100%" size="large" class="inputx" label="CVV2"
                                      placeholder="CVV2"
                                      v-model="editCard.cvv"/>
                        </div>
                        <div style="font-size: 32px;">
                            <vs-input style="width: 100%" class="inputx" size="large" label="تاریخ انقضا"
                                      placeholder="تاریخ انقضا"
                                      v-model="editCard.date"/>
                        </div>
                        <div style="font-size: 32px;">
                            <label for="logo">لوگو بانک</label>
                            <br>
                            <input type="file" @change="logoSelected" id="logo" required>
                        </div>
                    </div>
                    <div slot="footer">
                        <vs-row vs-justify="flex-end">
                            <vs-button color="success" @click="updateCard()" icon="save" class="vs-divider">ذخیره
                            </vs-button>
                        </vs-row>
                    </div>
                </vs-card>
            </vs-col>

            <vs-row vs-justify="center" v-if="cards.length > 0">
                <swiper class="swiper" :options="swiperOption" v-if="cards.length > 1 && (!addMode && !editMode)">
                    <swiper-slide v-for="(card, index) in cards"
                                  :key="index">
                        <vs-col type="flex" vs-justify="center" vs-align="center" vs-w="12" style="padding: 1em 2em">
                            <vs-card>
                                <div slot="header">
                                    <h3>
                                        {{ card.name }}
                                    </h3>
                                </div>
                                <div>
                                    <img :src="card.logo" alt="لوگو بانک">
                                    <br>
                                    <h1 style="text-align: center; font-size: 32px; padding: 2rem 0;">{{
                                        formatCardNumber(card.number)
                                        }}</h1>
                                    <br>
                                    <vs-row>
                                        <vs-col vs-type="flex" vs-justify="right" vs-align="center" vs-w="6">
                                            <span>{{ card.bank }}</span>
                                        </vs-col>
                                        <vs-col vs-type="flex" vs-justify="flex-end" vs-align="center" vs-w="6">
                                            <span>تاریخ انقضا : {{ card.date }}</span>
                                        </vs-col>
                                    </vs-row>
                                </div>
                                <div slot="footer">
                                    <vs-row vs-justify="flex-end">
                                        <vs-button @click="setEditCard(index)" color="primary" icon="edit"></vs-button>
                                        <div style="width: 15px"></div>
                                        <vs-button @click="deleteCard(card)" type="gradient" color="danger"
                                                   icon="delete"></vs-button>
                                    </vs-row>
                                </div>
                            </vs-card>
                        </vs-col>
                    </swiper-slide>
                </swiper>

                <vs-col v-if="cards.length <= 1 && (!addMode && !editMode)" v-for="(card, index) in cards"
                        :key="index" type="flex" vs-justify="center" vs-align="center" vs-w="12"
                        style="padding: 1em 2em">
                    <vs-card>
                        <div slot="header">
                            <h3>
                                {{ card.name }}
                            </h3>
                        </div>
                        <div>
                            <div style="display: flex; justify-content: center;padding: 1rem 0;">
                                <img :src="card.logo"
                                     style="width: 128px; height: auto; box-shadow: 0 3px 6px #00000030"
                                     alt="لوگو بانک">
                            </div>
                            <br>
                            <h1 style="text-align: center; font-size: 32px; padding-bottom: 1rem">{{
                                formatCardNumber(card.number)
                                }}</h1>
                            <br>
                            <vs-row>
                                <vs-col vs-type="flex" vs-justify="right" vs-align="center" vs-w="4">
                                    <span> نام بانک : {{ card.bank }}</span>
                                </vs-col>
                                <vs-col vs-type="flex" vs-justify="center" vs-align="center" vs-w="4" dir="rtl">
                                    <span>تاریخ انقضا : {{ card.date }}</span>
                                </vs-col>
                                <vs-col vs-type="flex" vs-justify="flex-end" vs-align="center" vs-w="4">
                                    <span>{{ card.cvv }} : CVV2</span>
                                </vs-col>
                            </vs-row>
                        </div>
                        <div slot="footer">
                            <vs-row vs-justify="flex-end">
                                <vs-button @click="setEditCard(index)" color="primary" icon="edit"></vs-button>
                                <div style="width: 15px"></div>
                                <vs-button @click="deleteCard(card)" type="gradient" color="danger"
                                           icon="delete"></vs-button>
                            </vs-row>
                        </div>
                    </vs-card>
                </vs-col>
            </vs-row>

            <div v-if="cards.length === 0 && !addMode" style="width: 100%; padding: 1rem 3rem">
                <vs-alert
                        color="primary"
                        title="کارتی یافت نشد" active="true">
                    لطفا یک کارت جدید اضافه کنید!
                </vs-alert>
            </div>
        </vs-row>
    </div>
</template>

<script>
    // @ is an alias to /src
    import fs from "fs";
    import {Swiper, SwiperSlide} from 'vue-awesome-swiper'
    import 'swiper/swiper-bundle.css'

    export default {
        name: 'Home',
        components: {
            Swiper,
            SwiperSlide
        },
        data: () => ({
            swiperOption: {
                slidesPerView: 1.2,
                spaceBetween: 0,
            },
            cards: [],
            addMode: false,
            editMode: false,
            hasError: false,
            newCard: {
                name: null,
                number: null,
                date: null,
                bank: null,
                cvv: null,
                logo: null,
            },
            editCardIndex: null,
            editCard: {
                name: null,
                number: null,
                date: null,
                bank: null,
                cvv: null,
                logo: null,
            },
        }),
        watch: {
            cards: {
                handler() {
                    localStorage.setItem('cards', JSON.stringify(this.cards))
                }
            }
        },
        methods: {
            logoSelected() {
                let vm = this;
                var file = document
                    .querySelector('input[type=file]')
                    .files[0];
                var reader = new FileReader();
                reader.onload = function (e) {
                    vm.newCard.logo = e.target.result;
                    vm.editCard.logo = e.target.result
                };
                reader.onerror = function (error) {
                    alert(error);
                };
                reader.readAsDataURL(file);
            },

            getBank() {
                const IRAN_BANK_INFO = [
                    {
                        name: 'eghtesad-novin',
                        fa_name: 'بانک اقتصاد نوین',
                        account_id: [627412],
                    },
                    {
                        name: 'ansar',
                        fa_name: 'بانک انصار',
                        account_id: [627381],
                    },
                    {
                        name: 'iranzamin',
                        fa_name: 'بانک ایران‌زمین',
                        account_id: [505785],
                    },
                    {
                        name: 'parsian',
                        fa_name: 'بانک پارسیان',
                        account_id: [622106, 639194, 627884],
                    },
                    {
                        name: 'pasargad',
                        fa_name: 'بانک پاسارگاد',
                        account_id: [639347, 502229],
                    },
                    {
                        name: 'ayandeh',
                        fa_name: 'بانک آینده',
                        account_id: [636214],
                    },
                    {
                        name: 'tejarat',
                        fa_name: 'بانک تجارت',
                        account_id: [585983, 627353],
                    },
                    {
                        name: 'tose-e-taavon',
                        fa_name: 'بانک توسعه‌تعاون',
                        account_id: [627884],
                    },
                    {
                        name: 'tose-e-saderat',
                        fa_name: 'بانک توسعه‌صادرات',
                        account_id: [627648, 207177],
                    },
                    {
                        name: 'hekmate-iranian',
                        fa_name: 'بانک حکمت‌ایرانیان',
                        account_id: [636949],
                    },
                    {
                        name: 'day',
                        fa_name: 'بانک دی',
                        account_id: [502938],
                    },
                    {
                        name: 'refah-e-kargaran',
                        fa_name: 'بانک رفاه‌کارگران',
                        account_id: [589463],
                    },
                    {
                        name: 'saman',
                        fa_name: 'بانک سامان',
                        account_id: [621986],
                    },
                    {
                        name: 'sepah',
                        fa_name: 'بانک سپه',
                        account_id: [589210],
                    },
                    {
                        name: 'sarmayeh',
                        fa_name: 'بانک سرمایه',
                        account_id: [639607],
                    },
                    {
                        name: 'sina',
                        fa_name: 'بانک سینا',
                        account_id: [639346],
                    },
                    {
                        name: 'shahr',
                        fa_name: 'بانک شهر',
                        account_id: [502806],
                    },
                    {
                        name: 'saderat',
                        fa_name: 'بانک صادرات‌ایران',
                        account_id: [603769],
                    },
                    {
                        name: 'sanat-o-maadan',
                        fa_name: 'بانک صنعت و معدن',
                        account_id: [627961],
                    },
                    {
                        name: 'mehr',
                        fa_name: 'بانک قرض‌الحسنه مهر',
                        account_id: [606373, 636795],
                    },
                    {
                        name: 'ghavamin',
                        fa_name: 'بانک قوامین',
                        account_id: [639599],
                    },
                    {
                        name: 'kar-afarin',
                        fa_name: 'بانک کارآفرین',
                        account_id: [627488, 502910],
                    },
                    {
                        name: 'keshavarzi',
                        fa_name: 'بانک کشاورزی',
                        account_id: [603770, 502910],
                    },
                    {
                        name: 'gardeshgari',
                        fa_name: 'بانک گردشگری',
                        account_id: [505416],
                    },
                    {
                        name: 'maskan',
                        fa_name: 'بانک مسکن',
                        account_id: [628023],
                    },
                    {
                        name: 'mellat',
                        fa_name: 'بانک ملت',
                        account_id: [610433, 991975],
                    },
                    {
                        name: 'melli',
                        fa_name: 'بانک ملی ایران',
                        account_id: [603799],
                    },
                    {
                        name: 'mehr-e-eghtesad',
                        fa_name: 'بانک مهراقتصاد',
                        account_id: [639370],
                    },
                    {
                        name: 'khavarmiane',
                        fa_name: 'بانک خاورمیانه',
                        account_id: [],
                    },
                    {
                        name: 'post',
                        fa_name: 'پست‌بانک ایران',
                        account_id: [627760],
                    },
                    {
                        name: 'tose-e',
                        fa_name: 'موسسه‌اعتباری توسعه',
                        account_id: [628157],
                    },
                    {
                        name: 'kosar',
                        fa_name: 'موسسه‌اعتباری کوثر',
                        account_id: [505801],
                    },
                ];
                this.cards.forEach(card => {
                    const bank = IRAN_BANK_INFO.filter(bank => {
                        return bank.account_id.includes(parseInt(card.number / Math.pow(10, Math.floor(Math.log10(card.number)) - 6 + 1)))
                    })[0];
                    if (!bank) {
                        card.bank = 'بانک پیدا نشد!'
                    } else {
                        card.bank = bank.fa_name
                    }
                });
            },
            formatCardNumber(num) {
                num = Array.from(String(num), Number).join("");
                return num.match(/.{1,4}/g).join("-")
            },
            saveCard() {
                if (this.newCard.name === null || this.newCard.number === null || this.newCard.date === null || this.newCard.number.length < 16) {
                    this.hasError = true;
                } else {
                    this.hasError = false;
                    this.cards.push(this.newCard);
                    this.getBank(this);
                    this.newCard = {
                        name: null,
                        number: null,
                        date: null,
                        bank: null,
                        cvv: null,
                        logo: null,
                    };
                    this.addMode = false;
                }
            },
            deleteCard(card) {
                this.cards.splice(this.cards.indexOf(card), 1)
            },

            setEditCard(index) {
                this.editMode = !this.editMode;
                this.editCardIndex = index;
                let card = this.cards[index];
                this.editCard = card;
            },

            updateCard() {
                this.deleteCard(this.cards[this.editCardIndex]);
                this.newCard = this.editCard;
                this.saveCard();
                this.editMode = false;
                this.editCard = {
                    name: null,
                    number: null,
                    date: null,
                    bank: null,
                    cvv: null,
                    logo: null,
                };
                this.editCardIndex = null;
            }
        },
        mounted() {
            if (localStorage.getItem('cards')) {
                this.cards = JSON.parse(localStorage.getItem('cards'));

                if (this.cards !== null) {
                    this.getBank(this);
                }
            }

        }
    }
</script>

<style>
    span {
        font-size: 13pt;
    }

    .con-vs-alert, .con-vs-alert-danger {
        height: auto;
    }

    label {
        font-size: 12pt;
        font-weight: 200;
    }
</style>