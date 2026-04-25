###### Concept

# data-bag

Sending, requesting and retrieving data can be managed easier if formalia are predictable and of low complexity. JSON became a big player less due its main purpose of serialisation, but rather by its main usage to transfer "self-containing" objects structures.

A "data-bag" is meant to be a most minimally predefined JSON object to simplify sending, requesting and retrieving data. It may be conceived as passing an arbitrary bag with some content accompanied by a note about what to do with the content on sending, being empty beside a note about what is demanded on requesting, and on being returned will be filled with a note about what was done or the things that were demanded and, in case, a note about problems with fulfilling the send or request wish.


## Sending data

  * To be sent from client to server
  * Defined to be a JSON object with properties `actions` and `data` with substructures depending on the backend to which data is sent, e.g.
    ```json
      {
        "actions": {
          "create": {
            "update_if_exist": true
          }
        },
        "data": {
          "name": {
            "first": "Joe",
            "last": "Doe"
          },
          "hobbies": [
            "Relaxing",
            "Hanging around",
            "Cruising around town"
          ]
        }
      }
    ```

## Requesting data

  * To be sent from client to server
  * Only restricted to be a JSON object, properties are to be defined according e.g. to the backend from which data is requested, e.g.
    ```json
      {
        "fields": {
          "name": {
            "format": "last, first"
          },
          "hobbies": {
            "maxcount": 2
          }
        }
      }
    ```



## Retrieving data

  * To be sent from server to client
  * Defined to be a JSON object with properties `data` and `error`
    * in TypeScript terms
        ```ts
          type data_bag_retrieve = {
            data: data_prop // see below,
            error: error_prop // see below
          }
        ```
    *  `error` is `null` if no error occurred, or an array with information about the error as described in TypeScript terms like
        ```ts
          type error_prop = no_error | error_info

          /** Denoting absence of errors*/
          type no_error = null

          /** Denoting occurrence of an error with information about the error scenario */
          type error_info = [
            error_id,
            dev_msg?,
            user_msg?
          ]

          /** Should allow to refer to an nambigously defined specific error scenario */
          type error_id = string

          /** Informal message / hint about error to be displayed to devs */
          type dev_msg = string

          /** Informal message / hint about error to be displayed to devs */
          type user_msg = string
        ```
    * `data` is `null` if nothing can be retrieved, or an object like the same property in a [sending bag](#sending-data)
      * both `null` and an object value have to be interpreted together with the value of `error` 
      * `data` in TypeScript terms
          ```ts
            type data_prop = null | Record<string, unknown>
          ```
