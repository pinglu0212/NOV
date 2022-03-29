# DMV Appointment Scheduler

## Challenge

Create an appointment scheduler application for the Department of Motor Vehicles (DMV).

### Data Set

* Customer records
  * 5,000 total records
  * Available data:
    * Id
    * Service type
    * Appointment duration
* Teller records
  * 150 total records
  * Available data:
    * Id
    * Specialty type
    * Multiplier

### Requirements

* The application should read in data for customers and assign each customer an appointment with a teller.
* A teller can have only one appointment at a time.
* When a customer's service type matches the specialty type of the assigned teller, the multiplier will be multiplied against the customer's appointment duration to reduce the appointment time.
* The customer's service type does not need to match the teller's specialty type (in fact, a customer's service type may not match any teller's specialty type).
* Use the data set provided. Do not alter the json data.
* You may use any programming language.

### Result

Our goal is to process all customers and have our tellers go home as early as possible. All the tellers will leave together once the last customer has been processed. Therefore, your results will be measured according to the duration of all appointments for the teller with the longest total duration.