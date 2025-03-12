# API Documentation

## API Summary

| Endpoint | Method | Description |
| --- | --- | --- |
| `/auth/sign-up/` | `POST` | Sign up and create a new account |
| `/auth/sign-in/` | `POST` | Sign in with credentials |
| `/auth/respond-to-challenge/` | `POST` | Respond to a new password required challenge |
| `/api/auth/password-reset` | `POST` | Request a password reset |
| `/api/wellness-plan/active` | `GET` | Get active wellness plan details |
| `/api/wellness-plan/{planId}` | `PUT` | Update wellness plan details |
| `/api/wellness-plan` | `POST` | Create a new wellness plan |
| `/api/client-values` | `GET` | List all client values |
| `/api/client-values` | `POST` | Create a new client value |
| `/api/client-values/{id}` | `GET` | Get client value by ID |
| `/api/client-values/{id}` | `PUT` | Update client value details |
| `/api/client-values/{id}` | `DELETE` | Delete client value by ID |
| `/api/patients/{patientId}/client-values` | `GET` | List all client values for a patient |
| `/api/patients/{patientId}/client-values` | `POST` | Add client value to a patient |
| `/api/patients/{patientId}/client-values/{valueId}` | `DELETE` | Remove client value from a patient |
| `/api/experimental-exercises` | `GET` | List experimental exercises recommended by physicians |
| `/api/experimental-exercises/{wellnessPlanId}/audio` | `PUT` | Remove recommended audio from experimental exercises |
| `/api/patients/{patientId}/profile` | `GET` | Get patient profile details |
| `/api/patients/{patientId}/profile` | `PUT` | Update patient profile details |
| `/api/patients/{patientId}/feedback` | `POST` | Provide feedback |
| `/api/patients/{patientId}/outcome-measures` | `GET` | List all previous outcome measures for a patient |
| `/api/outcome-measures` | `POST` | Create a new outcome measure |

## Authentication and Authorization

All endpoints require authentication using Bearer Token (JWT). Ensure that the token is included in the `Authorization` header of each request. Example:

```
Authorization: Bearer <your-token>
```



### Sign-Up

**URL:** `/auth/sign-up/`  
**Method:** `POST`  
**Description:** This endpoint allows a user to sign up and create a new account.

#### Request Body:

- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "email": "<string>"
    }
    ```

#### Responses:

##### If Sign-Up is Successful:

- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "message": "User created successfully and verification email sent",
        "user": {
            // ...user details...
        }
    }
    ```

##### If Sign-Up Fails:

- **Status Code:** `400 Bad Request`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "message": "Email is required."
    }
    ```

##### If Internal Server Error Occurs:

- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "detail": "An error message describing the issue"
    }
    ```

### Initial Sign-In

**URL:** `/auth/sign-in/`  
**Method:** `POST`  
**Description:** This endpoint allows a user to sign in with their credentials.

#### Request Body:

- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "email": "<string>",
        "password": "<string>"
    }
    ```

#### Responses:

##### If Sign-In is Successful:

- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "message": "Sign-in successful",
        "tokens": {
            // ...authentication tokens...
        }
    }
    ```

##### If New Password is Required:

- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "message": "New password required.",
        "challengeName": "NEW_PASSWORD_REQUIRED",
        "sessionId": "<string>"
    }
    ```

##### If Sign-In Fails:

- **Status Code:** `400 Bad Request`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "message": "Email and Password are required."
    }
    ```

##### If Internal Server Error Occurs:

- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "detail": "An error message describing the issue"
    }
    ```

### Respond to Challenge

**URL:** `/auth/respond-to-challenge/`  
**Method:** `POST`  
**Description:** This endpoint allows a user to respond to a new password required challenge.

#### Request Body:

- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "email": "<string>",
        "newPassword": "<string>",
        "sessionId": "<string>"
    }
    ```

#### Responses:

##### If Challenge Response is Successful:

- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "message": "Password updated successfully. Sign-in complete.",
        "tokens": {
            // ...authentication tokens...
        }
    }
    ```

##### If Challenge Response Fails:

- **Status Code:** `400 Bad Request`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "message": "Invalid or expired session ID."
    }
    ```

##### If Internal Server Error Occurs:

- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "detail": "An error message describing the issue"
    }
    ```


### Password Reset

**URL:** `/api/auth/password-reset`  
**Method:** `POST`  
**Description:** This endpoint allows a user to request a password reset.

#### Request Body:

- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "email": "<string>",
        "password": "<string>"
    }
    ```

#### Responses:

##### If Request is Successful:

- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 200,
        "data": null,
        "error": null,
        "message": "Password reset email sent"
    }
    ```

##### If Email Does Not Exist:

- **Status Code:** `404 Not Found`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Email not found"
        },
        "message": "Password reset failed"
    }
    ```

##### If Internal Server Error Occurs:

- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }
    ```

## Wellness Plan Endpoints

### Get Active Wellness Plan Details

**URL:** `/api/wellness-plan/active`  
**Method:** `GET`  
**Description:** This endpoint retrieves the details of the active wellness plan for the authenticated user.  
If no active wellness plan exists, the API **does not** return a `404 Not Found`. Instead, it responds with a structured object containing `null` values and `"status": "inactive"`.

### **Possible Status Values**

The `status` field in the response can have one of the following values:

- **`"active"`** → The wellness plan is currently in progress.  
- **`"cancelled"`** → No active wellness plan exists. The wellness plan was cancelled by the user or system.  
- **`"completed"`** → No active wellness plan exists. The wellness plan was successfully completed.

**Frontend Handling Recommendation:**  
- Always check the `"status"` field to determine the plan state.  
- Avoid relying on `404` responses—**the API always returns a structured response.**

**Authentication:** Requires Bearer Token (JWT)

---

### **Responses**

#### **If Active Wellness Plan Exists**

- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": "<integer>",
        "data": {
            "id": "<integer>",
            "patientId": "<integer>",
            "clientSessionId": "<integer>",
            "what": "<string>",
            "why": "<string>",
            "commitmentValue": "<integer>",
            "startDateTime": "<ISO 8601 string>",
            "additionalDateTime": ["<ISO 8601 string>"],
            "frequency": "<string>",
            "repeatIntervalUnit": "<string>",
            "repeatIntervalValue": "<integer>",
            "endAfterUnit": "<string>",
            "endAfterValue": "<integer>",
            "status": "<string>",
            "notificationSettings": [
                {
                    "remindMeFrequency": "<string>",
                    "notificationChannel": "<string>",
                    "notificationRecipient": "<string>"
                }
            ],
            "clientValueIds": ["<integer>"],
            "clientValues": ["<string>"]
        },
        "message": "<string>"
    }
    ```

#### **If No Active Wellness Plan Exists**

- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": "<integer>",
        "data": {
            "id": null,
            "patientId": null,
            "clientSessionId": null,
            "what": null,
            "why": null,
            "commitmentValue": null,
            "startDateTime": null,
            "additionalDateTime": [null],
            "frequency": null,
            "repeatIntervalUnit": null,
            "repeatIntervalValue": null,
            "endAfterUnit": null,
            "endAfterValue": null,
            "status": "inactive",
            "notificationSettings": [
                {
                    "remindMeFrequency": null,
                    "notificationChannel": null,
                    "notificationRecipient": null
                }
            ],
            "clientValueIds": [null],
            "clientValues": [null]
        },
        "message": "No active wellness plan exists"
    }
    ```

#### **If Internal Server Error Occurs**

- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": "<integer>",
        "data": {},
        "error": {
            "code": "<string>",
            "message": "<string>"
        },
        "message": "<string>"
    }
    ```

  ```
### Update Wellness Plan

**URL:** `/api/wellness-plan/{planId}`  
**Method:** `PUT`  
**Description:** This endpoint updates the details of a specific wellness plan by ID.

#### Request Body:

- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "clientSessionId": "<integer>",
        "what": "<string>",
        "why": "<string>",
        "commitmentValue": "<integer>",
        "startDateTime": "<ISO 8601 string>",
        "notificationSettings": [
            {
                "remindMeFrequency": "<integer>",
                "notificationChannel": "<integer>",
                "notificationRecipient": "<string>"
            }
        ],
        "additionalDateTime": [
            "<ISO 8601 string>"
        ],
        "frequency": "<integer>",
        "repeatIntervalUnit": "<integer>",
        "repeatIntervalValue": "<integer>",
        "endAfterUnit": "<integer>",
        "endAfterValue": "<integer>",
        "clientValueIds": [
            "<integer>"
        ]
    }
    ```

#### Input Validation:

- **planId:** Required, string
- **clientSessionId:** Required, integer
- **what:** Required, string
- **why:** Required, string
- **commitmentValue:** Required, integer
- **startDateTime:** Required, string, ISO 8601 format
- **notificationSettings:** Required, array of objects
- **additionalDateTime:** Required, array of strings, ISO 8601 format
- **frequency:** Required, integer
- **repeatIntervalUnit:** Required, integer
- **repeatIntervalValue:** Required, integer
- **endAfterUnit:** Required, integer
- **endAfterValue:** Required, integer
- **clientValueIds:** Required, array of integers

#### Responses:

##### If Update is Successful:

- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": "<integer>",
        "data": {
            "id": "<integer>",
            "patientId": "<integer>",
            "clientSessionId": "<integer>",
            "what": "<string>",
            "why": "<string>",
            "commitmentValue": "<integer>",
            "startDateTime": "<ISO 8601 string>",
            "additionalDateTime": ["<ISO 8601 string>"],
            "frequency": "<integer>",
            "repeatIntervalUnit": "<string>",
            "repeatIntervalValue": "<integer>",
            "endAfterUnit": "<string>",
            "endAfterValue": "<integer>",
            "status": "<string>",
            "notificationSettings": [
                {
                    "remindMeFrequency": "<string>",
                    "notificationChannel": "<string>",
                    "notificationRecipient": "<string>"
                }
            ],
            "clientValueIds": [ "<integer>"],
            "clientValues": ["<string>"]
        },
        "message": "<string>"
    }
    ```

##### If Wellness Plan Does Not Exist:

- **Status Code:** `404 Not Found`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": "<integer>",
        "data": {},
        "error": {
            "code": "<string>",
            "message": "<string>"
        },
        "message": "<string>"
    }
    ```

##### If Internal Server Error Occurs:

- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": "<integer>",
        "data": {},
        "error": {
            "code": "<string>",
            "message": "<string>"
        },
        "message": "<string>"
    }
    ```

### Create Wellness Plan

**URL:** `/api/wellness-plan`  
**Method:** `POST`  
**Description:** This endpoint creates a new wellness plan.

#### Request Body:

- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "clientSessionId": "<integer>",
        "what": "<string>",
        "why": "<string>",
        "commitmentValue": "<integer>",
        "startDateTime": "<ISO 8601 string>",
        "notificationSettings": [
            {
                "remindMeFrequency": "<integer>",
                "notificationChannel": "<integer>",
                "notificationRecipient": "<string>"
            }
        ],
        "additionalDateTime": [
            "<ISO 8601 string>"
        ],
        "frequency": "<integer>",
        "repeatIntervalUnit": "<integer>",
        "repeatIntervalValue": "<integer>",
        "endAfterUnit": "<integer>",
        "endAfterValue": "<integer>",
        "clientValueIds": [
            "<integer>"
        ]
    }
    ```

#### Input Validation:

- **planId:** Required, string
- **clientSessionId:** Required, integer
- **what:** Required, string
- **why:** Required, string
- **commitmentValue:** Required, integer
- **startDateTime:** Required, string, ISO 8601 format
- **notificationSettings:** Required, array of objects
- **additionalDateTime:** Required, array of strings, ISO 8601 format
- **frequency:** Required, integer
- **repeatIntervalUnit:** Required, integer
- **repeatIntervalValue:** Required, integer
- **endAfterUnit:** Required, integer
- **endAfterValue:** Required, integer
- **clientValueIds:** Required, array of integers

#### Responses:

##### If Update is Successful:

- **Status Code:** `201 Created`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": "<integer>",
        "data": {
            "id": "<integer>",
            "patientId": "<integer>",
            "clientSessionId": "<integer>",
            "what": "<string>",
            "why": "<string>",
            "commitmentValue": "<integer>",
            "startDateTime": "<ISO 8601 string>",
            "additionalDateTime": ["<ISO 8601 string>"],
            "frequency": "<integer>",
            "repeatIntervalUnit": "<string>",
            "repeatIntervalValue": "<integer>",
            "endAfterUnit": "<string>",
            "endAfterValue": "<integer>",
            "status": "<string>",
            "notificationSettings": [
                {
                    "remindMeFrequency": "<string>",
                    "notificationChannel": "<string>",
                    "notificationRecipient": "<string>"
                }
            ],
            "clientValueIds": [ "<integer>"],
            "clientValues": ["<string>"]
        },
        "message": "<string>"
    }
    ```

##### If Wellness Plan Does Not Exist:

- **Status Code:** `404 Not Found`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": "<integer>",
        "data": {},
        "error": {
            "code": "<string>",
            "message": "<string>"
        },
        "message": "<string>"
    }
    ```

##### If Internal Server Error Occurs:

- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": "<integer>",
        "data": {},
        "error": {
            "code": "<string>",
            "message": "<string>"
        },
        "message": "<string>"
    }
    ```
---
## Client Value Endpoints

### List All Client Values

**URL:** `/api/client-values`  
**Method:** `GET`  
**Description:** Retrieves a list of all available client values.

**Authentication:** Requires Bearer Token (JWT)

### **Responses**

#### **If Client Values Exist**
- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 200,
        "data": [
            {
                "id": "<integer>",
                "value": "<string>"
            }
        ],
        "error": null,
        "message": "Client values retrieved successfully"
    }
    ```

#### **If No Client Values Exist**
- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 200,
        "data": [],
        "error": null,
        "message": "No client values found"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }
    ```
### Create Client Value

**URL:** `/api/client-values`  
**Method:** `POST`  
**Description:** Creates a new client value.

**Authentication:** Requires Bearer Token (JWT)  

---

### **Request Body**
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "value": "<string>"
    }
    ```

### **Responses**

#### **If Client Value is Created Successfully**
- **Status Code:** `201 Created`
- **Body:**
    ```json
    {
        "status": 201,
        "data": {
            "id": "<integer>",
            "value": "<string>"
        },
        "error": null,
        "message": "Client value created successfully"
    }
    ```

#### **If Validation Fails**
- **Status Code:** `400 Bad Request`
- **Body:**
    ```json
    {
        "status": 400,
        "data": null,
        "error": {
            "code": "VALIDATION_ERROR",
            "message": "Value is required"
        },
        "message": "Invalid request data"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }

### Get Client Value by ID

**URL:** `/api/client-values/{id}`  
**Method:** `GET`  
**Description:** Retrieves a specific client value by ID.

**Authentication:** Requires Bearer Token (JWT)  

---

### **Responses**

#### **If Client Value Exists**
- **Status Code:** `200 OK`
- **Body:**
    ```json
    {
        "status": 200,
        "data": {
            "id": "<integer>",
            "value": "<string>"
        },
        "error": null,
        "message": "Client value retrieved successfully"
    }
    ```

#### **If Client Value Does Not Exist**
- **Status Code:** `404 Not Found`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Client value not found"
        },
        "message": "Requested client value does not exist"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }

### Update Client Value

**URL:** `/api/client-values/{id}`  
**Method:** `PUT`  
**Description:** Updates the details of a specific client value.

**Authentication:** Requires Bearer Token (JWT)  

---

### **Request Body**
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "value": "<string>"
    }
    ```

### **Responses**

#### **If Update is Successful**
- **Status Code:** `200 OK`
- **Body:**
    ```json
    {
        "status": 200,
        "data": {
            "id": "<integer>",
            "value": "<string>"
        },
        "error": null,
        "message": "Client value updated successfully"
    }
    ```

#### **If Client Value Does Not Exist**
- **Status Code:** `404 Not Found`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Client value not found"
        },
        "message": "Requested client value does not exist"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }


### Delete Client Value

**URL:** `/api/client-values/{id}`  
**Method:** `DELETE`  
**Description:** Deletes a specific client value by ID.

**Authentication:** Requires Bearer Token (JWT)  

---

### **Responses**

#### **If Deletion is Successful**
- **Status Code:** `200 OK`
- **Body:**
    ```json
    {
        "status": 200,
        "data": null,
        "error": null,
        "message": "Client value deleted successfully"
    }
    ```

#### **If Client Value Does Not Exist**
- **Status Code:** `404 Not Found`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Client value not found"
        },
        "message": "Requested client value does not exist"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }


---


### List All Client Values for a Patient

**URL:** `/api/patients/{patientId}/client-values`  
**Method:** `GET`  
**Description:** This endpoint retrieves all client values associated with a specific patient.

**Authentication:** Requires Bearer Token (JWT)  

### **Responses**

#### **If Client Values Exist**
- **Status Code:** `200 OK`
- **Body:**
    ```json
    {
        "status": 200,
        "data": {
            "patientId": "<string>",
            "clientValues": [
                {
                    "valueId": "<integer>",
                    "valueName": "<string>"
                }
            ]
        },
        "error": null,
        "message": "Client values retrieved successfully"
    }
    ```

#### **If Patient Does Not Exist or Has No Client Values**
- **Status Code:** `404 Not Found`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Patient or client values not found"
        },
        "message": "Requested patient does not exist or has no associated client values"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }
    ```

### Add Client Value to Patient

**URL:** `/api/patients/{patientId}/client-values`  
**Method:** `POST`  
**Description:** This endpoint adds a client value to a specific patient.

**Authentication:** Requires Bearer Token (JWT)  

### **Request Body**
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "valueId": "<integer>"
    }
    ```

### **Responses**

#### **If Addition is Successful**
- **Status Code:** `200 OK`
- **Body:**
    ```json
    {
        "status": 200,
        "data": {
            "patientId": "<string>",
            "valueId": "<integer>",
            "valueName": "<string>"
        },
        "error": null,
        "message": "Client value successfully added to patient"
    }
    ```

#### **If Patient or Client Value Does Not Exist**
- **Status Code:** `404 Not Found`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Patient or client value not found"
        },
        "message": "Requested patient or client value does not exist"
    }
    ```

#### **If Client Value is Already Associated**
- **Status Code:** `409 Conflict`
- **Body:**
    ```json
    {
        "status": 409,
        "data": null,
        "error": {
            "code": "CONFLICT",
            "message": "Client value is already associated with this patient"
        },
        "message": "Duplicate client value assignment is not allowed"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }
    ```

### Remove Client Value from Patient

The **DELETE API** for client values **only removes the association** between a patient and a client value.  
**It does not delete the client value itself** from the system. The same client value can be reassigned to the same or different patients in the future.

### **Key Points**
- **Removing a client value from a patient** means the patient no longer has that value linked to them.
- **The client value remains in the system** and can be reassigned to other patients later.
- **The operation is non-destructive**, affecting only the relationship between the patient and the value.


### **Example Scenarios**
1. ✅ *A patient is no longer interested in a particular client value → Remove it without affecting other patients who have the same value.*
2. ✅ *A client value is mistakenly assigned to a patient → Remove it, but the value remains available for reuse.*
3. ❌ *If a client value needs to be permanently removed from the system → That requires a different API for deleting client values entirely.*

#### **Remove Client Value from Patient**
- **Method:** `DELETE`
- **Endpoint:** `/api/patients/{patientId}/client-values/{valueId}`
- **Effect:** Removes the association but does not delete the client value.
- **Status Codes:**
    - `204 No Content` → Successfully removed.
    - `404 Not Found` → Patient or value does not exist.
    - `409 Conflict` → Value is not associated with the patient.
    - `500 Internal Server Error` → Unexpected issue.

### ✅ **Best Practice Reminder**
- **Use this API when unlinking a value from a patient.**
- **Do not use this API for deleting client values from the system.**

### **API Reference**
**URL:** `/api/patients/{patientId}/client-values/{valueId}`  
**Method:** `DELETE`  
**Description:** This endpoint removes a client value from a specific patient.

**Authentication:** Requires Bearer Token (JWT)  

### **Responses**

#### **If Removal is Successful**
- **Status Code:** `204 No Content**
- **Body:** *(Empty response body as per REST best practices for DELETE operations)*

#### **If Patient or Client Value Does Not Exist**
- **Status Code:** `404 Not Found`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Patient or client value not found"
        },
        "message": "Requested patient or client value does not exist"
    }
    ```

#### **If Client Value is Not Associated with Patient**
- **Status Code:** `409 Conflict`
- **Body:**
    ```json
    {
        "status": 409,
        "data": null,
        "error": {
            "code": "CONFLICT",
            "message": "Client value is not associated with this patient"
        },
        "message": "Cannot remove a client value that is not assigned"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }
    ```

---

## Experimental Exercises Endpoints

### List Experimental Exercises Recommended by Physicians

**URL:** `/api/experimental-exercises`  
**Method:** `GET`  
**Description:** This endpoint retrieves a list of experimental exercises recommended by physicians.

**Authentication:** Requires Bearer Token (JWT)  

---

### **Responses**

#### **If Experimental Exercises Exist**
- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 200,
        "data": {
            "wellnessPlanId": "<integer>",
            "audioKeys": "[\"<string>\", \"<string>\"]"
        },
        "error": null,
        "message": "Experimental exercises retrieved successfully"
    }
    ```

#### **If No Experimental Exercises Exist**
- **Status Code:** `404 Not Found`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "No experimental exercises found"
        },
        "message": "No experimental exercises were found in the system"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }
    ```

### Remove Recommended Audio from Experimental Exercises

**URL:** `/api/experimental-exercises/{wellnessPlanId}/audio`  
**Method:** `PUT`  
**Description:** This endpoint removes recommended audio from the experimental exercises associated with a specific `wellnessPlanId`.

**Authentication:** Requires Bearer Token (JWT)

---

### **Request Body**

- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "audioKeys": "[\"<string>\", \"<string>\"]"
    }
    ```

### **Responses**

#### **If Audio is Removed Successfully**
- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 200,
        "data": {
            "wellnessPlanId": "<integer>",
            "audioKeys": "[\"<string>\", \"<string>\"]"
        },
        "error": null,
        "message": "Audio removed successfully from experimental exercises"
    }
    ```

#### **If Wellness Plan Does Not Exist**
- **Status Code:** `404 Not Found`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Wellness plan not found"
        },
        "message": "The specified wellness plan does not exist"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }
    ```

---

## Patient Profile Endpoints

### Get Patient Profile

**URL:** `/api/patients/{patientId}/profile`  
**Method:** `GET`  
**Description:** This endpoint retrieves the profile details of a specific patient by ID.

**Authentication:** Requires Bearer Token (JWT)  

---

### **Responses**

#### **If Patient Profile Exists**
- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 200,
        "data": {
            "patientId": "<integer>",
            "preferredName": "<string>",
            "email": "<string>",
            "phone": "<string>"
        },
        "error": null,
        "message": "Patient profile retrieved successfully"
    }
    ```

#### **If Patient Profile Does Not Exist**
- **Status Code:** `404 Not Found`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Patient profile not found"
        },
        "message": "The requested patient profile does not exist"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }
    ```
---


### Update Patient Profile

**URL:** `/api/patients/{patientId}/profile`  
**Method:** `PUT`  
**Description:** This endpoint updates the profile details of a specific patient by ID.

**Authentication:** Requires Bearer Token (JWT)  

### **Request Body**

- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "patientId": "<integer>",
        "preferredName": "<string>",
        "email": "<string>",
        "phone": "<string>"
    }
    ```

### **Responses**

#### **If Update is Successful**
- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 200,
        "data": {
            "patientId": "<integer>",
            "preferredName": "<string>",
            "email": "<string>",
            "phone": "<string>"
        },
        "error": null,
        "message": "Patient profile updated successfully"
    }
    ```

#### **If Patient Profile Does Not Exist**
- **Status Code:** `404 Not Found`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Patient profile not found"
        },
        "message": "The requested patient profile does not exist"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }
    ```

---
## Feedback Endpoints

### Provide Feedback

**URL:** `/api/patients/{patientId}/feedback`  
**Method:** `POST`  
**Description:** This endpoint allows a patient to provide feedback.

**Authentication:** Requires Bearer Token (JWT)  

---

### **Request Body**

- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "feedback": "<string>"
    }
    ```

---

### **Responses**

#### **If Feedback is Submitted Successfully**
- **Status Code:** `201 Created`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 201,
        "data": {
            "feedbackId": "<string>",
            "patientId": "<string>",
            "feedback": "<string>",
            "submittedAt": "<string (ISO 8601 format)>"
        },
        "error": null,
        "message": "Feedback submitted successfully"
    }
    ```

#### **If Patient Does Not Exist**
- **Status Code:** `404 Not Found`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 404,
        "data": null,
        "error": {
            "code": "NOT_FOUND",
            "message": "Patient not found"
        },
        "message": "The requested patient was not found"
    }
    ```

#### **If Internal Server Error Occurs**
- **Status Code:** `500 Internal Server Error`
- **Body:**
    ```json
    {
        "status": 500,
        "data": null,
        "error": {
            "code": "INTERNAL_SERVER_ERROR",
            "message": "An unexpected error occurred"
        },
        "message": "Something went wrong. Please try again later."
    }
    ```
---

### List All Previous Outcome Measures for Patient

**URL:** `/api/patients/{patientId}/outcome-measures`  
**Method:** `GET`  
**Description:** This endpoint retrieves all the previous outcome measures (e.g., DUKE, Hua Oranga) for a specific patient by patient ID.

**Authentication:** Requires Bearer Token (JWT)

### Responses

#### If Outcome Measures Exist:
- **Status Code:** `200 OK`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
      "status": 200,
      "data": {
        "DUKE": [
          {
            "id": "integer",
            "measureCategory": "string",
            "sectionScore": [
              {
                "sectionName": "string",
                "score": "float"
              }
            ],
            "overallScore": "float",
            "createdDate": "string (ISO 8601 format)"
          }
        ],
        "Hua Oranga": [
          {
            "id": "integer",
            "measureCategory": "string",
            "sectionScore": [
              {
                "sectionName": "string",
                "score": "float"
              }
            ],
            "overallScore": "float",
            "createdDate": "string (ISO 8601 format)"
          }
        ]
      },
      "message": "Outcome measures retrieved successfully"
    }
    ```

#### If No Outcome Measures Exist for the Patient:
- **Status Code:** `404 Not Found`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
      "status": 404,
      "data": null,
      "error": {
        "code": "NOT_FOUND",
        "message": "Outcome measures not found for the patient"
      },
      "message": "No outcome measures exist for the specified patient."
    }
    ```

#### Response Schema:
- **DUKE**: Array of outcome measures with the following fields:
  - **id**: Unique identifier for the measure (integer).
  - **measureCategory**: The category of the measure (e.g., "DUKE").
  - **sectionScore**: Array of sections with corresponding scores.
    - **sectionName**: Name of the section (e.g., "Physical Health", "Mental Health", etc.).
    - **score**: The score for the section (float).
  - **overallScore**: The overall score of the outcome measure (float).
  - **createdDate**: The date when the outcome measure was created in ISO 8601 format (string).

- **Hua Oranga**: Array of outcome measures with the following fields:
  - **id**: Unique identifier for the measure (integer).
  - **measureCategory**: The category of the measure (e.g., "Hua Oranga").
  - **sectionScore**: Array of sections with corresponding scores.
    - **sectionName**: Name of the section (e.g., "Taha Tinana/Physical Health", "Taha Wairua/Spiritual Health", etc.).
    - **score**: The score for the section (float).
  - **overallScore**: The overall score of the outcome measure (float).
  - **createdDate**: The date when the outcome measure was created in ISO 8601 format (string).

---

### Create Outcome Measure

**URL:** `/api/outcome-measures`  
**Method:** `POST`  
**Description:** This endpoint creates a new outcome measure for a specific patient, using a measure category and associated answers.

---

### **Request Body:**

- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "measureCategoryId": "integer",
        "answers": [
            {
                "question": "string",
                "value": "string"
            }
        ]
    }
    ```

---

### **Responses:**

#### **If Creation is Successful:**
- **Status Code:** `201 Created`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "status": 200,
        "data": {
            "outcomeMeasureId": "integer",
            "measureCategory": {
                "id": "integer",
                "name": "string"
            },
            "answers": [
                {
                    "question": "string",
                    "value": "string"
                }
            ],
            "createdDate": "string (ISO 8601 format)"
        },
        "error": {},
        "message" : ""
    }
    ```

#### **If Measure Category Does Not Exist:**
- **Status Code:** `404 Not Found`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "error": {
            "message": "Measure category not found",
            "statusCode": 404
        }
    }
    ```

#### **If Invalid Data is Provided:**
- **Status Code:** `400 Bad Request`
- **Content-Type:** `application/json`
- **Body:**
    ```json
    {
        "error": {
            "message": "Invalid data provided",
            "statusCode": 400
        }
    }
    ```

---

### **Example Request:**
```json
{
    "measureCategoryId": 1,
    "answers": [
        {
            "question": "What is your physical health score?",
            "value": "80"
        },
        {
            "question": "What is your mental health score?",
            "value": "70"
        }
    ]
}



