# Javier Troya - Assignment SFE02 - Front-End Engineer

Quick commands to start the project

- The project was created using [Vuetify](https://vuetifyjs.com/).
- The components of the project are:
  - [Dialogs](https://vuetifyjs.com/en/components/dialogs/#usage)
  - [Cards](https://vuetifyjs.com/en/components/cards/#usage)
  - [Inputs](https://vuetifyjs.com/en/components/inputs/#usage)
  - [Buttons](https://vuetifyjs.com/en/components/buttons/#usage)
  - [Alerts](https://vuetifyjs.com/en/components/alerts/#usage)
  - [Grid](https://vuetifyjs.com/en/components/grids/#usage)
- The style of the Input number was created using the prepend and append slots
- The `FlightAmountEditor` component emits an event that updates the parent view

Tasks performed:
- [x] The agent will be able to add or reduce quota for a specific subscriber when needed by using a simple interface with two controls (quota field and reason field).
- [x] The agent performing this action will not be able to add or reduce quota without selecting a reason.
- [x] When the agent adds quota, they should be able to see the following options in the 	reason field: 'Subscriber canceled flight', ‘Airline canceled flight', ‘Customer compensation' or  ’Other'.
- [x] When the agent removes quota, they should be able to see the following options in the “reason” field: 'Flight not redeposited after a flight cancellation', ‘Subscriber had log in or password issues', ‘Subscriber had issues when booking', ‘Subscription has not renewed correctly', ‘Other'.
- [x] The save button will be only active when the quota has been changed and the reason has been selected.
- [x] The agent will not be able to add quota for a subscriber higher than 3 flights.
- [x] The agent will not be able to remove quota for a subscriber lower than 0.
- [x] When the X (close) button is clicked it should close the modal and no change should be applied.
- [x] When the save button is clicked it should save the changes and display a contextual success / error message.

`git clone git@github.com:phycticio/caravelo.git testing-caravelo && cd testing-caravelo && nvm use && yarn && yarn dev`