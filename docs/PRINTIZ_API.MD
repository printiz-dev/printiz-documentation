# Printiz API
The **Printiz API** allows developers to integrate with the Printiz platform for managing users, products, and orders.  
All requests are JSON over HTTPS, and all responses are JSON encoded.

---
## General Information

1. [Introduction](#printiz-api)
2. [Base URL](#base-url)
3. [APIs](#apis-specification)
    - [Login API](#login)
    - [Create Orders API](#create-orders)
    - [Get Orders API](#get-orders)

---

## Base URL
https://printiz.us/

---

## APIs Specification

All requests require authentication via a **JWT Bearer Token** from Login API.  
Use the **Login API** to obtain a token, then include it in the `Authorization` header.

Authorization: Bearer [your-token-goes-here]

### Login
**POST** `/users/auth/login`

#### Request
```json
{
  "username": "seller@printiz",
  "password": "123456"
}
```
#### Response
```json
{
    "success": true,
    "message": "Login successful",
    "data": {
        "token": "******",
        "token_type": "Bearer",
        "user": {
            "id": 2,
            "name": "Seller User",
            "email": "seller@printiz.com",
            "role": "seller",
            "avatar": null,
            "status": 1,
            "coin_balance": 0.0,
            "is_active": true,
            "role_label": "Seller",
            "type": 1,
            "permissions": [],
            "carrier_name": null,
            "created_at": "2025-07-12T05:05:10Z",
            "updated_at": "2025-09-21T08:00:55Z"
        }
    }
}
```


### Create Orders
**POST** `/api/orders/create-orders`

#### Request
```json
{
  "orders": [
        {
            "order_unique_code": "TT_24US_NY213",
            "external_id": "577038693459857585",
            "customer_name": "Reagan Martin",
            "customer_email": "v4bD3X66J3IAHK5CG5PUN6NW33W24@scs.tiktokw.us",
            "customer_contact": "(+1)8703230639",
            "delivery_street": "4120 Cypress Knoll Dr",
            "delivery_city": "Jonesboro",
            "delivery_state": "Arkansas",
            "delivery_zip_code": "72405",
            "delivery_country": "US",
            "delivery_unit": "",
            "platform": "TIKTOK",
            "tracking_number": "",
            "delivery_label": "",
            "items": [
                {
                    "quantity": 1,
                    "note": "",
                    "size": "XL",
                    "color": "SAND",
                    "shirt_type": "Tshirt",
                    "front_design": "https://s3.amazonaws.com/az89ecom.image/66914_front.png",
                    "back_design": "https://s3.amazonaws.com/az89ecom.image/66914_front.png",
                    "mockup_front": "https://s3.amazonaws.com/az89ecom.image/66914_front.png",
                    "mockup_back": "https://s3.amazonaws.com/az89ecom.image/66914_front.png",
                    "extra_design_1": null,
                    "mockup_extra_1": null,
                    "extra_design_2": null,
                    "mockup_extra_2": null,
                    "extra_design_3": null,
                    "mockup_extra_3": null,
                    "is_rush": "no"
                }
            ]
        },
        {
            "order_unique_code": "TT_24US_NY213_01",
            "external_id": "5770386934598575851",
            "customer_name": "Reagan Martin",
            "customer_email": "v4bD3X66J3IAHK5CG5PUN6NW33W24@scs.tiktokw.us",
            "customer_contact": "(+1)8703230639",
            "delivery_street": "4120 Cypress Knoll Dr",
            "delivery_city": "Jonesboro",
            "delivery_state": "Arkansas",
            "delivery_zip_code": "72405",
            "delivery_country": "US",
            "delivery_unit": "",
            "platform": "TIKTOK",
            "tracking_number": "123456789",
            "delivery_label": "https://drive.google.com/file/d/1QU_u1IPmPsNdRKfRVUWmma3S5V_RxNvP/view?usp=sharing",
            "items": [
                {
                    "quantity": 2,
                    "note": "",
                    "size": "L",
                    "color": "SAND",
                    "shirt_type": "kidshirt",
                    "front_design": "https://s3.amazonaws.com/az89ecom.image/66914_front.png",
                    "back_design": "https://s3.amazonaws.com/az89ecom.image/66914_front.png",
                    "mockup_front": "https://s3.amazonaws.com/az89ecom.image/66914_front.png",
                    "mockup_back": "https://s3.amazonaws.com/az89ecom.image/66914_front.png",
                    "extra_design_1": "https://s3.amazonaws.com/az89ecom.image/66914_front.png",
                    "mockup_extra_1": "https://s3.amazonaws.com/az89ecom.image/66914_front.png",
                    "extra_design_2": null,
                    "mockup_extra_2": null,
                    "extra_design_3": null,
                    "mockup_extra_3": null,
                    "is_rush": "yes"
                }
            ]
        }
  ]
}
```
#### Response
```json
{
    "message": "Bulk order creation completed. Created: 2, Updated: 0, Errors: 0",
    "total_processed": 2,
    "success_count": 2,
    "error_count": 0,
    "created_count": 2,
    "updated_count": 0,
    "results": [
        {
            "index": 0,
            "order_unique_code": "TT_24US_NY213",
            "success": true,
            "message": "Order created successfully",
            "order_id": 1,
            "updated": false
        },
        {
            "index": 1,
            "order_unique_code": "TT_24US_NY213_01",
            "success": true,
            "message": "Order created successfully",
            "order_id": 2,
            "updated": false
        }
    ]
}
```

### Get Orders
**GET** `/api/orders/`
#### Params for lookup order by id(s) or unique code(s)
Single Search
</br>
`/api/orders/search=23-13452-90981`
</br>
Multiple Search
</br>
`/api/orders/search=23-13452-90981 21-13455-64028`
#### Request
```json
{}
```
#### Response
```json
{
    "success": true,
    "message": "Orders retrieved successfully",
    "data": {
        "data": [
            {
                "id": 81,
                "order_unique_code": "13-13468-53887",
                "external_id": "",
                "order_number": null,
                "invoice_number": null,
                "order_type": "normal",
                "status": "Order_Await_Approval",
                "business_id": "2",
                "business_name": "Printiz",
                "business_currency_code": "USD",
                "business_address": null,
                "customer_name": "Marc H. Falco",
                "customer_contact": "+1 610-666-7070",
                "customer_email": "",
                "existing_customer_id": null,
                "delivery_unit": "",
                "delivery_street": "1004 Shearwater Dr",
                "delivery_city": "Audubon",
                "delivery_state": "PA",
                "delivery_zip_code": "19403-2012",
                "delivery_country": "US",
                "delivery_note": null,
                "delivery_fee": 2.79,
                "service_fee": 0.0,
                "delivery_label": "",
                "subtotal": 1.0,
                "total": 3.79,
                "discount": 0.0,
                "tax": 0.0,
                "platform": "FACEBOOK",
                "platform_business_id": "2",
                "created_at_epoch": 1758443070,
                "scheduled_complete_at_epoch": 0,
                "order_note": null,
                "tracking_number": "",
                "assigned_printer_id": null,
                "pushed_to_printer_at": null,
                "printer_response_at": null,
                "printer_notes": null,
                "printer_status": null,
                "user_id": 2,
                "item_quantity": 0,
                "payment_method_id": null,
                "shipping_method_id": null,
                "order_tracking_status_id": null,
                "delivery_date": null,
                "order_date": "2025-09-21T08:22:55",
                "status_id": 1,
                "description": null,
                "user_created_id": 2,
                "user_updated_id": 2,
                "user_owner_id": 2,
                "created_at": null,
                "updated_at": "2025-09-21T08:24:48",
                "carrier_name": null,
                "shipping_status": null,
                "shipped_at": null,
                "order_items": [
                    {
                        "id": 271,
                        "order_id": 81,
                        "product_id": null,
                        "product_name": "Tshirt",
                        "quantity": 1,
                        "unit_price": 0.0,
                        "total": 1.0,
                        "note": "",
                        "variant_id": null,
                        "size": "M",
                        "color": "Red",
                        "shirt_type": "Tshirt",
                        "front_design": "https://lh3.googleusercontent.com/d/1nlVfwUwcAARBfBW4rhpGu5bvaoM8MYIf",
                        "back_design": "https://lh3.googleusercontent.com/d/1CrYULEKggx5t_M0BQprs1o-0uB7n862r",
                        "mockup_front": "https://i.ebayimg.com/images/g/jT8AAeSwlaZokJQQ/s-l1600.jpg",
                        "mockup_back": "https://i.ebayimg.com/images/g/jT8AAeSwlaZokJQQ/s-l1600.jpg",
                        "extra_design_1": null,
                        "mockup_extra_1": null,
                        "extra_design_2": null,
                        "mockup_extra_2": null,
                        "extra_design_3": null,
                        "mockup_extra_3": null,
                        "is_rush": false,
                        "created_at": null,
                        "updated_at": null
                    }
                ],
                "order_detail": {
                    "id": 211,
                    "order_id": 81,
                    "customer_source_id": null,
                    "order_creator_id": null,
                    "order_note": null,
                    "shipping_method": "",
                    "link_label": "",
                    "note": "",
                    "order_change_actions": {},
                    "additional_params": {
                        "priority": "normal",
                        "special_instructions": ""
                    },
                    "status": 1,
                    "description": null,
                    "user_created_id": 2,
                    "user_updated_id": 2,
                    "user_owner_id": 2,
                    "created_at": null,
                    "updated_at": null
                },
                "user": null,
                "created_by": {
                    "id": 2,
                    "name": "Seller User",
                    "email": "seller@printiz.com"
                },
                "updated_by": {
                    "id": 2,
                    "name": "Seller User",
                    "email": "seller@printiz.com"
                },
                "owner": {
                    "id": 2,
                    "name": "Seller User",
                    "email": "seller@printiz.com"
                },
                "status_label": "Awaiting Approval",
                "formatted_total": "3.79",
                "formatted_subtotal": "1.00",
                "formatted_discount": "0.00",
                "formatted_tax": "0.00",
                "formatted_delivery_fee": "2.79",
                "formatted_service_fee": "0.00",
                "items_count": 1,
                "full_delivery_address": "1004 Shearwater Dr, Audubon, PA, 19403-2012, US",
                "order_paged_sheet_url": "",
                "has_tasks": false
            }
        ],
        "meta_data": {
            "total": 130,
            "current_page": 1,
            "per_page": 20,
            "last_page": 7
        },
        "count": {
            "count_orders": 130,
            "count_total_items": 166
        }
    }
}
```
