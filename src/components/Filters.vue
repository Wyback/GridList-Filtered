
<template>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="http://www.w3schools.com/lib/w3.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
    <a href="#" v-on:click="showFilters"><md-icon class="fa fa-bars w3-text-teal" ></md-icon></a>
        <div class="titleBar">
            <div class="filtertitle" placeholder="">{{ title }}</div>
        </div>
        <div class="container">
            <div class="sidenav">
                <hr>
                <button class="dropdown-btn">Sexe
                    <i class="fa fa-caret-down"></i>
                </button>
                <div class="dropdown-container">
                    <div v-for="gender in genderFilters" :key="gender">
                            <input type="checkbox" :id="gender" :value="gender" v-model="checkedFilter" @change="filterSport">
                            <span>{{ gender }}</span>
                    </div>
                </div>
                <hr>
                <button class="dropdown-btn">Prix
                    <i class="fa fa-caret-down"></i>
                </button>
                <div class="dropdown-container">
                    <div v-for="price in priceFilters" :key="price">
                        <input type="checkbox" :id="price" :value="price" v-model="checkedFilter" @change="filterSport">
                        <span>{{ price }}</span>
                    </div>
                </div>
                <hr>
                <button class="dropdown-btn">Couleur
                    <i class="fa fa-caret-down"></i>
                </button>
                <div class="dropdown-container">
                    <div v-for="color in colorFilters" :key="color">
                        <input type="checkbox" :id="color" :value="color" v-model="checkedFilter" @change="filterSport">
                        <span>{{ color }}</span>
                    </div>
                </div>
                <hr>
                <button class="dropdown-btn">Sport
                    <i class="fa fa-caret-down"></i>
                </button>
                <div class="dropdown-container">
                    <div v-for="sport in sportFilters" :key="sport">
                        <input type="checkbox" :id="sport" :value="sport" v-model="checkedFilter" @change="filterSport">
                        <span>{{ sport }}</span>
                    </div>
                </div>
            </div>
            <div class="grid">
                <div v-for="product in products" :key="product.photo">
                <div class="card">
                    <img :src="require('@/assets/' + product.photo)"/>
                    <p><b>{{ product.article }}</b></p>
                    <p>{{ product.sexe }}</p>
                    <p>{{ product.couleur }}</p>
                    <p><b>{{ product.prix }}</b></p>
                </div>
                </div>
            </div>
        </div>
</template>

<script>
    import json from "../produits.json";
    import __ from 'lodash';

    const defaultTitle = "Chaussures"

    const genderFilters = [ 
        "Homme", 
        "Femme", 
        "Mixte"
    ];
    const priceFilters = [ 
        "50", 
        "50/100", 
        "100/150", 
        "150"
    ];
    const colorFilters = [ 
        "rose", 
        "noir", 
        "rouge", 
        "orange", 
        "bleu", 
        "blanc", 
        "marron", 
        "vert", 
        "jaune", 
        "gris"
    ];
    const sportFilters = [ 
        "Football", 
        "Basket", 
        "Running"
    ];
    

    export default {
        data() {
            return {
                genderFilters: genderFilters,
                priceFilters: priceFilters,
                colorFilters: colorFilters,
                sportFilters: sportFilters,
                checkedFilter: [],
                products: json,
                allProducts: json,
                title: defaultTitle,
                isFiltersShow: 0,
            };
        },
        methods: {
            filterGender: function() {
                const genderCommon = __.intersection(genderFilters,this.checkedFilter)
                if(
                    genderCommon.includes("Homme") && 
                    genderCommon.length == 1
                ) {
                    this.title = "Chaussures pour Homme"
                    this.products = this.allProducts.filter(function(item){
                        return item.sexe == "Homme";         
                    })
                }else if (
                    genderCommon.includes("Femme") && 
                    genderCommon.length == 1
                ) {
                    this.title = "Chaussures pour Femme"
                    this.products = this.allProducts.filter(function(item){
                        return item.sexe == "Femme";         
                    })
                }else if (
                    genderCommon.includes("Mixte") && 
                    genderCommon.length == 1
                ) {
                    this.title = "Chaussures Mixte"
                    this.products = this.allProducts.filter(function(item){
                        return item.sexe == "Mixte";         
                    })
                } else if(genderCommon.length > 1){
                    this.title = "Chaussures"
                    this.products = [];
                    for(let element = 0; element < genderCommon.length; element++){
                        console.log(genderCommon[element])
                        this.products = this.products.concat(this.allProducts.filter(function(item){
                                    return item.sexe == genderCommon[element];
                            })
                        )
                    }
                }else {
                    this.title = "Chaussures"
                    this.products = this.allProducts
                }

                return this.products
            },
            filterPrice: function() {
                const priceCommon = __.intersection(priceFilters,this.checkedFilter)

                let filterData = this.filterGender()
                let onFilterPrice = []

                if(
                    priceCommon.includes("50") && 
                    priceCommon.length == 1
                ) {
                    this.title += " Less than 50 e"

                        this.products = filterData.filter(function(item){
                            return parseFloat(item.prix) < 50;     
                        })
                }else if (
                    priceCommon.includes("50/100") && 
                    priceCommon.length == 1
                ) {
                    this.title += " Between 50 e & 100 e"
                        this.products = filterData.filter(function(item){
                            return parseFloat(item.prix) >= 50 && parseFloat(item.prix) <= 100;     
                        })
                }else if (
                    priceCommon.includes("100/150") && 
                    priceCommon.length == 1
                ) {
                    this.title += " Between 100 e & 150 e"
                        this.products = filterData.filter(function(item){
                            return parseFloat(item.prix) > 100 && parseFloat(item.prix) <= 150;     
                        })
                }else if (
                    priceCommon.includes("150") && 
                    priceCommon.length == 1
                ) {
                    this.title += " More than 150 e"
                        this.products = filterData.filter(function(item){
                            return parseFloat(item.prix) > 150     
                        })
                } else if(priceCommon.length > 1){
                    this.title += ""
                    for(let element = 0; element < priceCommon.length; element++){
                        onFilterPrice = onFilterPrice.concat(filterData.filter(function(item){
                                if(priceCommon[element] === "50"){
                                    return parseFloat(item.prix) < 50;
                                }else if(priceCommon[element] === "50/100"){
                                    return parseFloat(item.prix) >= 50 && parseFloat(item.prix) <= 100;   
                                }else if(priceCommon[element] === "100/150"){
                                    return parseFloat(item.prix) > 100 && parseFloat(item.prix) <= 150;   
                                }else if(priceCommon[element] === "150"){
                                    return parseFloat(item.prix) > 150;   
                                }
                        }))
                    }
                    this.products = onFilterPrice
                }else {
                    this.title += ""
                    this.products = filterData
                }

                return this.products
            },
            filterColor: function() {
                const ColorCommon = __.intersection(colorFilters,this.checkedFilter)

                let filterData = this.filterPrice()
                let onFilterColor = []

                if(
                    ColorCommon &&
                    ColorCommon.length == 1
                ) {
                    this.title += ` ${ColorCommon}`
                    this.products = filterData.filter(function(item){
                        return item.couleur.includes(ColorCommon[0]) 
                    })
                }else if(ColorCommon.length > 1){
                    this.title += ''
                    for(let element = 0; element < ColorCommon.length; element++){
                        onFilterColor = onFilterColor.concat(filterData.filter(function(item){
                            if(!onFilterColor.includes(item)){
                                return item.couleur.toLowerCase().includes(ColorCommon[element]) 
                            }
                        }))
                        console.log(onFilterColor)
                    }
                    this.products = onFilterColor
                }else{
                    this.title += ''
                    this.products = filterData
                }
                return this.products

            },
            filterSport: function() {
                const SportCommon = __.intersection(sportFilters,this.checkedFilter)

                let filterData = this.filterColor()
                let onFilterSport = []

                if(
                    SportCommon &&
                    SportCommon.length == 1
                ) {
                    this.title += ` ${SportCommon[0]}`
                    this.products = filterData.filter(function(item){
                        return item.sport.includes(SportCommon[0]) 
                    })
                }else if(SportCommon.length > 1){
                    this.title += ''
                    for(let element = 0; element < SportCommon.length; element++){
                        onFilterSport = onFilterSport.concat(filterData.filter(function(item){
                            if(!onFilterSport.includes(item)){
                                return item.sport.includes(SportCommon[element]) 
                            }
                        }))
                        console.log(onFilterSport)
                    }
                    this.products = onFilterSport
                }else{
                    this.title += ''
                    this.products = filterData
                }
                return this.products

            },
            showFilters: function() {
                    if (!this.isFiltersShow) {
                        console.log("passing to filter")
                        document.getElementsByClassName("grid")[0].style.width = "0%"
                        document.getElementsByClassName("sidenav")[0].style.width = "80%"

                        document.getElementsByClassName("sidenav")[0].style.visibility = "visible"
                        document.getElementsByClassName("grid")[0].style.visibility = "hidden"
                        this.isFiltersShow = 1
                    } else {
                        console.log("passing to grid")
                        document.getElementsByClassName("grid")[0].style.width = "auto"
                        document.getElementsByClassName("sidenav")[0].style.width = "0%"

                        document.getElementsByClassName("sidenav")[0].style.visibility = "hidden";
                        document.getElementsByClassName("grid")[0].style.visibility = "visible";
                        this.isFiltersShow = 0
                    }
            }
        },
        mounted() {
            var dropdown = document.getElementsByClassName("dropdown-btn");
            var i;

            for (i = 0; i < dropdown.length; i++) {
                dropdown[i].addEventListener("click", function() {
                    this.classList.toggle("active");
                    var dropdownContent = this.nextElementSibling;
                    if (dropdownContent.style.display === "block") {
                        dropdownContent.style.display = "none";
                    } else {
                        dropdownContent.style.display = "block";
                    }
                });
            }
        }
    };
</script>

<style lang="scss" scoped>

.titleBar {
    display: flex;
        padding-bottom: 5%;
}

span {
    padding-left: 10px;
}

a{
    font-size: 100px;
    position: fixed;
    right: 180px;
}

hr {
    width: 90%;
}

.container {
    display: flex;
    align-items: flex-start;
    width: max-content;
}
.filtertitle {
  text-align: left;
  font-size: 3em;
  padding: 12px;
  padding-left: 35px;
  width: max-content;
}

/* ======= Grid ==================*/

  img{
    width: 100%;
  }
  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 25px;
    font-size: 2em;

    p {
        line-height: 0.5;
    }
  }
  .card{
    width: 100%;
  }
  @media screen and (max-width: 768px){
    .grid{
      grid-template-columns: 1fr 1fr;
    }

    @media screen and (max-width: 320px){
    .grid{
      grid-template-columns: 1fr;
      p {
          line-height: 1;
      }
    }
  }
  }
/*=================================*/

/* ======= Filters ==================*/
li {
    text-align: left;
    text-decoration: none;
    font-size: 2em;
}

/* Fixed sidenav, full height */
.sidenav {
  width: 33%;
  z-index: 1;
  top: 0;
  left: 0;
  background-color: #fff;
  overflow-x: hidden;
  padding: 20px;
  display: inline-block;
}

/* Style the sidenav links and the dropdown button */
.sidenav a, .dropdown-btn {
  padding: 6px 8px 6px 16px;
  text-decoration: none;
  font-size: 30px;
  color: black;
  display: block;
  border: none;
  background: none;
  width:100%;
  text-align: left;
  cursor: pointer;
  outline: none;
}

/* Main content */
.main {
  margin-left: 200px; /* Same as the width of the sidenav */
  font-size: 30px; /* Increased text to enable scrolling */
  padding: 0px 10px;
}

/* Dropdown container (hidden by default). Optional: add a lighter background color and some left padding to change the design of the dropdown content */
.dropdown-container {
  display: none;
  padding-left: 15px;
  text-align: initial;
}

/* Optional: Style the caret down icon */
.fa-caret-down {
  float: right;
  padding-right: 8px;
}

.fa-bars {
    visibility: hidden;
    margin-top: 6rem;
}

.grid p {
    text-align:left;
}

@media screen and (max-width: 2560px){
    .sidenav{
      width: 12%;
    }
    .grid{
        width: 88%;
    }
  }

  @media screen and (max-width: 2000px){
    .sidenav{
      width: 16%;
    }
    .grid{
        width: 90%;
    }
  }

@media screen and (max-width: 1440px){
    .sidenav{
      width: 12%;
    }
    .grid{
        width: 80%;
    }
  }

@media screen and (max-width: 1024px){
    .sidenav{
      width: 12%;
    }
    .grid{
        width: 80%;
    }
  }

  @media screen and (max-width: 768px){
    .titleBar {
        font-size: 3em;
    }
    .sidenav{
      visibility: hidden;
      width: 0%;
      font-size: 3em;
    }
    .grid{
        width: 89%;
        font-size: 5em;
    }
    .dropdown-btn {
        font-size: 1em;
    }
    .fa-bars {
        visibility: visible;
    }
  }

  @media screen and (max-width: 425px){
    .grid{
        width: 55%;
    }
  }

  @media screen and (max-width: 375px){
    .grid{
        width: 45%;
    }
  }

  @media screen and (max-width: 325px){
    .grid{
        width: 40%;
    }
  }

/*===============================*/
</style>