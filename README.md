function myFunction() {
  let ui = SpreadsheetApp.getUi();
  ui.createMenu('Weather')
    .addItem('Display Weather', 'getWeather')
    .addToUi();
}

function getWeather() {
  const API_KEY = '562014f242a5469489b184328241207';
  let url = 'http://api.weatherapi.com/v1/current.json?key=';

  let sheet = SpreadsheetApp.getActiveSheet();
  let location = sheet.getRange('B1').getValue();

  let request = url + API_KEY + '&q=' + location;
  let response = UrlFetchApp.fetch(request);
  let data = JSON.parse(response.getContentText());

  let weatherData = [];
  weatherData.push(data.current.temp_c);
  weatherData.push(data.current.temp_f);
  weatherData.push(data.current.condition.text);

  let weather = [];
  weather.push(weatherData);

  let icon = data.current.condition.icon;
  let targetRange = sheet.getRange('A4:C4');
  targetRange.setValues(weather);

  let iconRange = sheet.getRange('D4');
  iconRange.setFormula('=IMAGE("' + icon + '")');
}
