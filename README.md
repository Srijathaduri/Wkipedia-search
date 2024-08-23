# Wkipedia-search
Developed a Wikipedia search application using HTML, CSS, and JavaScript. The application allows users to search for articles on Wikipedia, displaying results dynamically with a clean and responsive

HTML CODE

<html>

<head>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous" />
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js" integrity="sha384-B4gt1jrGC7Jh4AgTPSdUtOBvfO8shuf57BaghqFfPlYxofvL8/KUEfYiJOMMV+rV" crossorigin="anonymous"></script>
    <script src="https://kit.fontawesome.com/5f59ca6ad3.js" crossorigin="anonymous"></script>
</head>

<body>
    <div class="main-container">
        <div class="wiki-search-header text-center">
            <img class="wiki-logo" src="https://nkb-backend-otg-media-static.s3.ap-south-1.amazonaws.com/ccbp-dynamic-webapps/wiki-logo-img.png" />
            <br />
            <input placeholder="Type a keyword and press Enter to search" type="search" class="search-input w-100" id="searchInput" />
        </div>
        <div class="d-none" id="spinner">
            <div class="d-flex justify-content-center">
                <div class="spinner-border" role="status">
                    <span class="sr-only">Loading...</span>
                </div>
            </div>
        </div>
        <div class="search-results" id="searchResults"></div>
    </div>
</body>





</html> 

CSS CODE 

@import url("https://fonts.googleapis.com/css2?family=Bree+Serif&family=Caveat:wght@400;700&family=Lobster&family=Monoton&family=Open+Sans:ital,wght@0,400;0,700;1,400;1,700&family=Playfair+Display+SC:ital,wght@0,400;0,700;1,700&family=Playfair+Display:ital,wght@0,400;0,700;1,700&family=Roboto:ital,wght@0,400;0,700;1,400;1,700&family=Source+Sans+Pro:ital,wght@0,400;0,700;1,700&family=Work+Sans:ital,wght@0,400;0,700;1,700&display=swap");

.main-container {
    font-family: "Roboto";
}

.wiki-search-header {
    border-width: 1px;
    border-style: solid;
    border-color: #d5cdcd;
    padding-top: 30px;
    padding-right: 20px;
    padding-bottom: 30px;
    padding-left: 20px;
    margin-bottom: 40px;
}

.wiki-logo {
    margin-bottom: 30px;
    width: 150px;
}

.search-input {
    border-radius: 3px;
    padding: 10px;
    border-width: 1px;
    border-style: solid;
    border-color: #d5cdcd;
    font-size: 18px;
}

.search-results {
    width: 100%;
    padding-left: 20px;
}

.result-item {
    margin-bottom: 20px;
}

.result-title {
    font-size: 22px;
}

.link-description {
    font-size: 15px;
    color: #444444;
}

.result-url {
    color: #006621;
    text-decoration: none;
} 

JAVA SCRIPT 
let searchInputEl = document.getElementById("searchInput");
let searchResultsEl = document.getElementById("searchResults");
let spinnerEl = document.getElementById("spinner");

function createAndAppendSearchResult(result) {
    //create result item in container
    let resultEl = document.createElement("div");
    resultEl.classList.add("result-item"); //adding styling
    searchResultsEl.appendChild(resultEl);



    //creating Search result title in container
    //<a class="result-item" href=",.,.,.,.," target="_blank">Rahul</a> in static form convert into dynamic form
    let {
        link,
        title,
        description
    } = result; //link title are keys on console.log(jsonData) so di-structuring
    let resultTitleEl = document.createElement("a"); //creating anchor element
    //assing the html attributes 
    resultTitleEl.href = link;
    resultTitleEl.target = "_blank";
    resultTitleEl.textContent = title;
    resultTitleEl.classList.add("result-title");
    resultEl.appendChild(resultTitleEl);




    //Follow content after title should come in Nxt line so for that Creating  Break Element </br>
    let titleBreakEl = document.createElement('br');
    resultEl.appendChild(titleBreakEl);






    //Creating URL Element
    //we need url so we need again anchor element 
    //<a class="result-url" href=",.,.,.,.," target="_blank">some link </a> in static form convert into dynamic form
    let urlEl = document.createElement("a"); //creating anchor element
    urlEl.href = link;
    urlEl.target = "_blank";
    urlEl.textContent = link;
    urlEl.classList.add("result-url");
    resultEl.appendChild(urlEl);

    //Before Discription we need to create break element so Create Break Element
    let linkBreakEl = document.createElement('br');
    resultEl.appendChild(linkBreakEl);

    //Add Discription
    let descriptionEl = document.createElement('p');
    descriptionEl.classList.add("link-description");
    descriptionEl.textContent = description;
    resultEl.appendChild(descriptionEl);
}



function displayResult(searchResults) {
    //displaying single search result 
    spinnerEl.classList.toggle("d-none");
    for (let result of searchResults) {
        createAndAppendSearchResult(result);
    }


}




function searchWikipedia(event) {
    if (event.key === "Enter") {
        let searchInput = searchInputEl.value; //Accessing what user has entered
        spinnerEl.classList.toggle("d-none");
        searchResultsEl.textContent = "";
        //fetch request
        let url = "https://apis.ccbp.in/wiki-search?search=" + searchInput;
        let options = { //fetching second step is acceing the configaration through object name options
            method: "GET" //to read web resource step is methos

        };
        fetch(url, options)
            .then(function(response) { //acceing the response here responce is argument
                return response.json(); //it is in JSON formate as obajet with value and key
            })
            .then(function(jsonData) { //here search input which is in the form of object is return as argument
                let {
                    search_results
                } = jsonData //here search result is key and value in array when we used console.log(jsonData).Now array is stored in search data 
                displayResult(search_results); //to display the result by function calling
            });
    }
}
searchInputEl.addEventListener("keydown", searchWikipedia);
