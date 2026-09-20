### DAILY WEATHER SKILL
Provides forecast for the weather either current or for the day.

## What does the agent need?
- Check my timezone specified on USER.md

## What actions can take
- Verify the current weather
- Check the weather for the day based on the location and time zone specified  on USER.md

## Success definition
- When current weather was asked provides the temperature in Celcius degrees, a summarizing weather title such as "Mostly cloudy" and some brief details of the current weather.
- When  weather for the day was asked provides the temperature in Celcius degrees, a summarizing weather title such as "Mostly cloudy" and some brief details of the current weather.

## Other skills required
- None

### DAILY SPENDING REGISTER
Registers the daily spending provided in a google spreadsheet file using composio.

## What does the agent need?
- API TOKEN for Composio stored in the .env file
- Configured access to Google Drive with composio
- Read and modify permits to file "Mi Plantilla - Presupuesto Mensual.xlsx"

## What actions can take
- Given a spending, register spending inside the Mi Plantilla - Presupuesto Mensual.xlsx file under the specified sheet.
- Registers spending on the given category
- Registers spending on the given amount
- Registers spending under the given spending name
- Duplicates the last sheet on the xlsx. file under the following convention: "Month-Year" 

## Success definition
- Registers the spendings on the xlsx. file "Mi Plantilla - Presupuesto Mensual.xlsx" under the specified sheet ("Month-Year")
- Duplicates the specified sheet to start registering spendings when requested
- Registers correctly the category of spending using the category labels in file
- Registers correctly the spending under the given name
- Registers the spending correctly udner the given amount

## Other skills required
- None

### DAILY LEARNING LOG
Registers the daily learning as few bullet points in a google doc file.

## What does the agent need?
- API TOKEN for Composio stored in the .env file
- Configured access to Google Drive with composio.

## What actions can take
- Register in a google doc the daily learning provided as few bullet points

## Success definition
- Registers the learning as few bullet points categorizing the learning with a title
## Other skills required
- None