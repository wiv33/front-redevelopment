<template>
  <v-item-group>
    <v-container>
      <v-row>
        <v-col>
          <v-card>
            <v-card-title>
              <span class="title font-weight-light">매입 시 투자금</span>
            </v-card-title>
            <v-card-text>
              <v-card-subtitle> 조합원 분양가</v-card-subtitle>

              <v-text-field style="color: white; font-weight: bold"
                            v-model:value="priceOfSale"
                            v-on:keydown.enter="updatePrice('priceOfSale', priceOfSale)"
              />
            </v-card-text>
            <v-card-text>
              <v-card-subtitle>감정평가액</v-card-subtitle>

              <v-text-field style="color: white; font-weight: bold"
                            v-model:value="appraisedPrice"
                            v-on:keydown.enter="updatePrice('appraisedPrice', appraisedPrice)"
              />
            </v-card-text>
            <v-card-text>
              <v-card-subtitle>비례율</v-card-subtitle>

              <v-text-field style="color: white; font-weight: bold"
                            v-model:value="relativePercent"
                            v-on:keydown.enter="updatePrice('relativePercent', relativePercent)"
              />
            </v-card-text>
            <v-card-text>
              <v-card-subtitle>프리미엄</v-card-subtitle>

              <v-text-field style="color: white; font-weight: bold"
                            v-model:value="premium"
                            v-on:keydown.enter="updatePrice('premium', premium)"
              />
            </v-card-text>
            <v-card-text>
              <v-card-subtitle>전세 보증금</v-card-subtitle>

              <v-text-field style="color: white; font-weight: bold"
                            v-model:value="deposit"
                            v-on:keydown.enter="updatePrice('deposit', deposit)"
              />
            </v-card-text>
            <v-card-text>
              <v-card-subtitle>이주비 대출금</v-card-subtitle>

              <v-text-field style="color: white; font-weight: bold"
                            v-model:value="migrantLoan"
                            v-on:keydown.enter="updatePrice('migrantLoan', migrantLoan)"
              />
            </v-card-text>
            <v-card-text>
              <v-card-subtitle>이사비</v-card-subtitle>

              <v-text-field style="color: white; font-weight: bold"
                            v-model:value="movingExpenses"
                            v-on:keydown.enter="updatePrice('movingExpenses', movingExpenses)"
              />
            </v-card-text>
            <v-card-text>
              <v-card-subtitle>조합원분양 계약금</v-card-subtitle>

              <v-text-field style="color: white; font-weight: bold"
                            v-model:value="unionMemberPrice"
                            v-on:keydown.enter="updatePrice('unionMemberPrice', unionMemberPrice)"
              />
            </v-card-text>
          </v-card>
        </v-col>

        <v-col>
          <v-card>
            <v-card-title>매입 시 투자금</v-card-title>
            <v-card-text>
              <v-row>
                <v-col>권리가액</v-col>
                <v-col>{{ amountOfRight }}</v-col>
              </v-row>
              <v-row>
                <v-col>프리미엄</v-col>
                <v-col>{{ premium }}</v-col>
              </v-row>
              <v-row>
                <v-col>전세보증금</v-col>
                <v-col>{{ deposit }}</v-col>
              </v-row>
              <v-row>
                <v-col>총 금액</v-col>
                <v-col>{{ firstResult() }}</v-col>
              </v-row>
            </v-card-text>
          </v-card>

          <v-card>
            <v-card-title>이주 시 필요한 투자금</v-card-title>
            <v-card-text>
              <v-row>
                <v-col>이주비 대출</v-col>
                <v-col>{{ migrantLoan }}</v-col>
              </v-row>
              <v-row>
                <v-col>이사비</v-col>
                <v-col>{{ movingExpenses }}</v-col>
              </v-row>
              <v-row>
                <v-col>전세보증금</v-col>
                <v-col>{{ deposit }}</v-col>
              </v-row>
              <v-row>
                <v-col>총 금액</v-col>
                <v-col>{{ secondResult() }}</v-col>
              </v-row>

            </v-card-text>
          </v-card>
          <v-card>
            <v-card-title>조합원 분양 시 필요한 투자금</v-card-title>
            <v-card-text>
              <v-row>
                <v-col>조합원 분양가</v-col>
                <v-col>{{ priceOfSale }}</v-col>
              </v-row>
              <v-row>
                <v-col>권리가액</v-col>
                <v-col>{{ premium }}</v-col>
              </v-row>
              <v-row>
                <v-col>조합원 분담금</v-col>
                <v-col>{{ unionMemberDividePrice }}</v-col>
              </v-row>
            </v-card-text>
          </v-card>
          <v-card>
            <v-card-title>총 투자금</v-card-title>
            <v-card-text>
              <v-row>
                <v-col>매입 투자금</v-col>
                <v-col>{{ firstPrice }}</v-col>
              </v-row>
              <v-row>
                <v-col>이주 시 필요한 투자금</v-col>
                <v-col>{{ secondPrice }}</v-col>
              </v-row>
              <v-row>
                <v-col>조합원 분양 시 필요한 투자금 * 10%</v-col>
                <v-col>{{ visitPrice(unionMemberDividePrice / 10) }}</v-col>
              </v-row>
              <v-row>
                <v-col>총 금액</v-col>
                <v-col>{{ finalPrice() }}</v-col>
              </v-row>

            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-item-group>
</template>

<script>

export default {
  name: "PurchaseInvestment",
  data() {
    return {
      dataKeys: ['priceOfSale', 'appraisedPrice', 'relativePercent', 'premium', 'deposit', 'migrantLoan', 'movingExpenses', 'unionMemberPrice', 'amountOfRight'],
      //조합원 분양가
      priceOfSale: 0, // 조합원 분양가
      appraisedPrice: 0,  // 감정평가액
      relativePercent: 0,  // 비례율
      premium: 0, // 프리미엄 금액
      deposit: 0, // 전세보증금
      migrantLoan: 0,  // 이주비 대출금
      movingExpenses: 0, // 이사비
      unionMemberPrice: 0,  // 조합원 분양 계약금
      amountOfRight: 0, // 권리가액
      unionMemberDividePrice: 0, // 조합원 분담금

      firstPrice: 0,
      secondPrice: 0,
      totalPrice: 0,
    }
  },
  mounted() {
    this.initData();
    this.visitPrices();
  },
  watch: {},
  computed: {},
  methods: {
    initData() {
      this.priceOfSale = 300000000; // 조합원 분양가
      this.appraisedPrice = 100000000;  // 감정평가액
      this.relativePercent = 100;  // 비례율
      this.premium = 100000000;
      this.deposit = 50000000;
      this.movingExpenses = 5000000; // 이사비
      this.migrantLoan = this.appraisedPrice * 0.4;
      this.unionMemberPrice = 0
      this.amountOfRight = this.appraisedPrice * this.relativePercent / 100
      this.unionMemberDividePrice = this.priceOfSale - this.amountOfRight;
    },
    removeComma: function (price) {
      return Number(String(price).replace(/,/gi, ''));
    }, updatePrice(key, price) {
      console.log(key, price);
      const removeComma = this.removeComma(price);
      console.log(removeComma);
      this[key] = removeComma;

      if (key === 'appraisedPrice') {
        this.migrantLoan = this.removeComma(this.appraisedPrice) * 0.4;
        this.calcAmountOfRight();
      }

      if (key === 'relativePercent') {
        this.calcAmountOfRight()
      }

      this.unionMemberDividePrice = this.removeComma(this.priceOfSale) - this.removeComma(this.amountOfRight);

      this.visitPrices();
      this.firstResult();
    },
    calcAmountOfRight() {
      this.amountOfRight = this.removeComma(this.appraisedPrice) *
        this.removeComma(this.relativePercent) / 100
    },
    visitPrice(price) {
      return Number(price).toLocaleString();
    },
    visitPrices: function () {
      this.dataKeys.forEach(v => this[v] = this.visitPrice(this.removeComma(this[v])));
    },
    firstResult: function () {
      this.firstPrice = this.removeComma(this.amountOfRight) +
        this.removeComma(this.premium) -
        this.removeComma(this.deposit);

      return this.visitPrice(this.firstPrice);
    },
    secondResult() {
      this.secondPrice = this.removeComma(this.migrantLoan) +
        this.removeComma(this.movingExpenses) -
        this.removeComma(this.deposit);

      return this.visitPrice(this.secondPrice);
    },
    finalPrice() {
      this.totalPrice = this.firstPrice + this.secondPrice + this.removeComma(this.unionMemberDividePrice / 10)
      return this.visitPrice(this.totalPrice);
    }
  }
}
</script>

<style scoped>

</style>
