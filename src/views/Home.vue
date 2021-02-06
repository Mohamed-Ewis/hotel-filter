<template>
  <div class="home">
    <div class="container my-4">
      <div class="row">
        <div class="col-4 dt-container">
          <label for="date-from" class="text-capitalize ">form :</label>
          <input type="date" id="date-from" v-model="dateFrom" />
        </div>
        <div class="col-4 dt-container">
          <label for="date-to" class="text-capitalize ">to :</label>
          <input type="date" id="date-to" v-model="dateTo" />
        </div>
        <div class="col-4">
          <button class="text-capitalize search ">
            search
          </button>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-4"></div>
        <div class="col-4">
          <p>total night : 5</p>
        </div>
        <div class="col-4 d-flex justify-content-between">
          <a class="sort" @click="sortName">sort by name</a>
          <a class="sort" @click="sortPrice">sort by price</a>
        </div>
      </div>
      <div class="row pt-4">
        <div class="col-4">
          <div class="mb-3">
            <input
              type="text"
              class="search-field"
              v-model="inputSearch"
              placeholder="Hotel Name"
              ref="input"
            />
          </div>
          <div class="">
            <h5 class="text-left">
              price filter
            </h5>
            <input
              type="range"
              class="form-range w-100"
              :min="minPrice"
              :max="maxPrice"
              v-model="rangePrice"
              id=""
            />
          </div>
        </div>
        <div class="col-8">
          <div class="row">
            <div class="col-6 " v-for="(hotel, i) in finalData" :key="i">
              <div class="card px-3 mb-3 ">
                <h6 class="text-capitalize">
                  <strong>Name :</strong> {{ hotel.name }}
                </h6>
                <p class="text-capitalize">
                  <strong>price :</strong> {{ hotel.price }}
                </p>
                <p class="text-uppercase">
                  <strong class="text-capitalize">city :</strong>
                  {{ hotel.city }}
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  components: {},
  data() {
    return {
      dateFrom: "2020-01-01",
      dateTo: "2020-01-01",
      hotelData: [],
      hotels: [
        {
          name: "Kempinski Hotel Mall of the Emirates",
          price: "200",
          city: "dubai",
          available_on: "2020-10-21"
        },
        {
          name: "Address Dubai Mall",
          price: "250",
          city: "dubai",
          available_on: "2020-10-15"
        },
        {
          name: "JW Marriott Marquis Hotel Dubai",
          price: "225",
          city: "dubai",
          available_on: "2020-09-21"
        },
        {
          name: "Hilton Dubai Al Habtoor City",
          price: "275",
          city: "dubai",
          available_on: "2020-10-25"
        },
        {
          name: "Sofitel Dubai Downtown",
          price: "300",
          city: "dubai",
          available_on: "2020-09-20"
        },
        {
          name: "Renaissance Downtown Hotel",
          price: "200",
          city: "dubai",
          available_on: "2020-10-23"
        },
        {
          name: "Sofitel Dubai Jumeirah Beach",
          price: "250",
          city: "dubai",
          available_on: "2020-09-20"
        },
        {
          name: "Ramada Downtown Dubai",
          price: "225",
          city: "dubai",
          available_on: "2020-09-25"
        },
        {
          name: "Sheraton Mall of the Emirates Hotel",
          price: "325",
          city: "dubai",
          available_on: "2020-10-24"
        },
        {
          name: "Emirates Grand Hotel Apartments",
          price: "300",
          city: "dubai",
          available_on: "2020-10-27"
        }
      ],
      inputSearch: "",
      allData: true,
      searchdData: false,
      rangePrice: null
    };
  },
  computed: {
    filterData: {
      get() {
        return this.hotels.filter(hotel => {
          return (
            hotel.available_on >= this.dateFrom &&
            hotel.available_on <= this.dateTo
          );
        });
      },
      set(val) {
        return val;
      }
    },
    finalData() {
      return this.filterData.filter(item => {
        return (
          item.name.toLowerCase().match(this.inputSearch) &&
          item.price > this.rangePrice
        );
      });
    },
    minPrice() {
      if (this.filterData.length > 0) {
        let filteredPrice = this.filterData.sort((a, b) => a.price - b.price);
        return filteredPrice[0].price;
      }
    },
    maxPrice() {
      if (this.filterData.length > 0) {
        let filteredPrice = this.filterData.sort((a, b) => a.price - b.price);
        return filteredPrice[filteredPrice.length - 1].price;
      }
    }
  },
  watch: {},
  created() {},
  methods: {
    getData() {
      this.$axios.get("http://www.mocky.io/v2/5eb8fcb12d0000d088357f2a");
      this.hotelData = response.data;
    },
    sortName() {
      this.filterData.sort((a, b) => a.name - b.name);
      console.log(
        this.filterData.sort((a, b) =>
          a.name < b.name ? -1 : a.name > b.name ? 1 : 0
        )
      );
    },
    sortPrice() {
      let data = this.filterData.sort((a, b) => a.price - b.price);
      // this.$set(this.filterData, data);
      console.log(this.filterData);
    }
  }
};
</script>

<style scoped>
.dt-container {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
button.search {
  width: 120px;
  height: 40px;
  border: 1px solid #ccc;
  border-radius: 10px;
  font-weight: 700;
  background: #2196f3;
  color: #fff;
  display: block;
  margin: 0 0 0 auto;
}
.dt-container {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
a.sort {
  width: auto;
  height: 40px;
  padding: 0 5px;
  border: 1px solid #ccc;
  border-radius: 10px;
  font-weight: 700;
  background: #2196f3;
  color: #fff;
  text-transform: capitalize;
  line-height: 40px;
  text-decoration: none;
  cursor: pointer;
}
.card {
  padding: 10px;
  border-radius: 10px;
  box-shadow: 0 0 5px #ccc;
  background: #f1f1f1;
}
h6,
p {
  text-align: left;
}
.search-field,
.search-field:focus {
  width: 100%;
  height: 40px;
  border-radius: 10px;
  border: 1px solid #ccc;
}
</style>
