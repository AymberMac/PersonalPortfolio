# Aymber McElroy Assignment 3: Build a Personal Portfolio Website

## Goal: Create a fully functional website that demonstrates my understanding of HTML, CSS, JavaScript, DOM manipulation, JSON, and XML.

## Requirements:
### 1. HTML: created a minimum of 5 distinct web pages.
#### Home: Landing page to personal portfolio with photo, name and position.
#### About: Information about myself including skills, education and awards.
#### Geospatial Portfolio: Showcase of current and pervious geospatial projects.
#### Additional Page: Information and links to my favorite GIS websites and resources.
#### Contact: A contact form.

### 2. CSS: Designed a responsive layout to ensure the site looks good on diverse platforms with styles to enhance the visual appeal of the site including: 
#### - color schemes, spacing, transitions, transformations, and button styles with hover responses

### 3. JavaScript: Implemented interactive features including:
#### - interactive active tab elements
            var tablinks = document.getElementsByClassName("tab-links");
            var tabcontents = document.getElementsByClassName("tab-contents");
            
            function opentab(tabname){
                for(tablink of tablinks){
                    tablink.classList.remove("active-link");
                }

                for(tabcontent of tabcontents){
                    tabcontent.classList.remove("active-tab");
                }

                event.currentTarget.classList.add("active-link");
                document.getElementById(tabname).classList.add("active-tab");

            }

#### - form validation
            const scriptURL = 'https://script.google.com/macros/s/AKfycbxnoTHR1IZ1wScWc1VB2MCWN8uLBiHkMgdtLsthBKku4p0tx4JzVEuzyvNMZ2x7fbQ/exec'
            const form = document.forms['submit-to-google-sheet']
            const msg = document.getElementById("msg")
          
            form.addEventListener('submit', e => {
              e.preventDefault()
              fetch(scriptURL, { method: 'POST', body: new FormData(form)})
                .then(response => {msg.innerHTML = "Message sent successfully"
                    setTimeout(function(){
                        msg.innerHTML = ""
                    },5000)
                    form.reset()
                })
                .catch(error => console.error('Error!', error.message))
            })

### 4. DOM: Used DOM manipulation to update the content based on user actions or data from JSON/XML
#### - interactive active tab elements (above)
#### - form validation (above)
#### - contact form data (below)

### 5. JSON: Created JSON files to store data related to contact form in GoogleSheets document 
var sheetName = 'Sheet1'
var scriptProp = PropertiesService.getScriptProperties()

function intialSetup () {
  var activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
  scriptProp.setProperty('key', activeSpreadsheet.getId())
}

function doPost (e) {
  var lock = LockService.getScriptLock()
  lock.tryLock(10000)

  try {
    var doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
    var sheet = doc.getSheetByName(sheetName)

    var headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0]
    var nextRow = sheet.getLastRow() + 1

    var newRow = headers.map(function(header) {
      return header === 'timestamp' ? new Date() : e.parameter[header]
    })

    sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])

    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow }))
      .setMimeType(ContentService.MimeType.JSON)
  }

  catch (e) {
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e }))
      .setMimeType(ContentService.MimeType.JSON)
  }

  finally {
    lock.releaseLock()
  }
}

### 6. XML: create an XML file for additional contact data
<?xml version="1.0" encoding="UTF-8"?>
<contacts>
    <contacts id="101">
        <name>John Doe</name>
        <email>jdoe@gmail.com</email>
        <message>You're hired!</message>
    </contacts>
    <contacts id="102">
        <name>Jane Doe</name>
        <email>janed@gmail.com</email>
        <message>I love University of Wyoming!</message>
    </contacts>
    <contacts id="103">
        <name>Johnny Doe Jr.</name>
        <email>jr@gmail.com</email>
        <message>Thank you!</message>
    </contacts>
</contacts>

### 7. Additional features: contact form with JavaScript validation when recieved