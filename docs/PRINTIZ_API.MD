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
    - [List Products API](#list-products)
---

## Base URL
https://printiz.app/

---

## APIs Specification

All requests require authentication via a **JWT Bearer Token** from Login API.  
Use the **Login API** to obtain a token, then include it in the `Authorization` header.

Authorization: Bearer [your-token-goes-here]

### Login
**POST** `api/users/auth/login`

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
**POST** `/api/orders/v1/create-orders`

#### Request
```json
{
  "orders": [
        {
            "order_unique_code": "AZBV_TT0811_007_577036687762100371",
            "external_id": "577036687762100371",
            "order_type": "label",
            "status": "Order_Await_Approval",
            "customer_name": "T***as D***ll",
            "customer_email": "v4bD2USHHNJ5RYNGGVP4MQ6C33W24@scs.tiktokw.us",
            "customer_contact": "(+1)501*****46",
            "delivery_street": "42************",
            "delivery_city": "N ***********",
            "delivery_state": "Arkansas",
            "delivery_zip_code": "72***",
            "delivery_country": "US",
            "delivery_unit": "",
            "subtotal": 10.5,
            "total": 11.0,
            "delivery_fee": 0.5,
            "service_fee": 0,
            "platform": "TIKTOK",
            "platform_business_id": "2",
            "created_at_epoch": 1755147628,
            "scheduled_complete_at_epoch": 0,
            "tracking_number": "9400109208316578510797",
            "delivery_label": "https://open-fs.tiktokshops.us/wsos_v2/oec_fulfillment_doc_tts/object/wsos6875cd8983598b37?expire=1752637210&skipCookie=true&timeStamp=1752550810&sign=4441f3e1b06a2f3f06aa85d9dd94051f36d2170a4b88778239e872fd0a2d8dd2",
            "items": [
                {
                    "item_unique_code": "AZBV_TT0811_007_577036687762100371_1755147628",
                    "product_unique_code": "TSHIRTGILDAN5000_2XL_SAND",
                    "product_code": "shirtVFbHFh",
                    "product_name": "Tshirt",
                    "quantity": 1,
                    "unit_price": 7.0,
                    "total": 10.5,
                    "note": "",
                    "size": "2XL",
                    "color": "SAND",
                    "shirt_type": "Tshirt",
                    "front_design": "https://s3.amazonaws.com/az89ecom.image/47370_front.png",
                    "back_design": "https://s3.amazonaws.com/az89ecom.image/47370_back.png",
                    "mockup_front": "https://p16-oec-ttp.tiktokcdn-us.com/tos-useast5-i-omjb5zjo8w-tx/5ad8d3eb559f4040b32e5e942eb9f23f~tplv-omjb5zjo8w-origin-jpeg.jpeg?dr=10493&from=520841845&idc=useast5&ps=933b5bde&shcp=2c1af732&shp=1f0b6a75&t=555f072d",
                    "mockup_back": "https://p16-oec-ttp.tiktokcdn-us.com/tos-useast5-i-omjb5zjo8w-tx/5ad8d3eb559f4040b32e5e942eb9f23f~tplv-omjb5zjo8w-origin-jpeg.jpeg?dr=10493&from=520841845&idc=useast5&ps=933b5bde&shcp=2c1af732&shp=1f0b6a75&t=555f072d",
                    "extra_design_1": null,
                    "mockup_extra_1": null,
                    "extra_design_2": null,
                    "mockup_extra_2": null,
                    "extra_design_3": null,
                    "mockup_extra_3": null,
                    "is_rush": false
                }
            ],
            "order_detail": {
                "shipping_method": "",
                "link_label": "https://open-fs.tiktokshops.us/wsos_v2/oec_fulfillment_doc_tts/object/wsos6875cd8983598b37?expire=1752637210&skipCookie=true&timeStamp=1752550810&sign=4441f3e1b06a2f3f06aa85d9dd94051f36d2170a4b88778239e872fd0a2d8dd2",
                "note": "",
                "order_change_actions": [],
                "additional_params": {
                    "priority": "normal",
                    "special_instructions": ""
                }
            },
            "order_change_actions": [
                {
                    "action_type": "Order_Await_Approval",
                    "at_epoch_second": 1755147628,
                    "description": "Order has been uploaded from .csv and awaits approval.",
                    "changed_by": "Seller-API-2"
                }
            ]
        }
    ]
}
```
#### Response
```json
{
    "response_code": 200,
    "success_orders": [
        {
            "order_unique_code": "AZBV_TT0811_007_577036687762100371",
            "status": "Order_Await_Approval"
        }
    ],
    "error_orders": [],
    "failed_orders": [],
    "total_requested_count": 1,
    "total_imported_count": 1
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

### List Products
**GET** `/api/products/`
#### Request
```json
{}
```
#### Response
```json
{
    "product_detail_list": [
        {
            "id": "fulltshirt_xs_black",
            "code": "",
            "name": "FULL TSHIRT (LARGE IMAGE)",
            "size": "xs",
            "color": "black",
            "base_cost": 0.0,
            "double_sided_printing": 3.5,
            "special_printing": 2.79,
            "first_item_shipping": 4.99,
            "additional_item_shipping": 2.5
        },
        {
            "id": "fulltshirt_s_black",
            "code": "",
            "name": "FULL TSHIRT (LARGE IMAGE)",
            "size": "s",
            "color": "black",
            "base_cost": 4.8,
            "double_sided_printing": 3.5,
            "special_printing": 2.79,
            "first_item_shipping": 4.99,
            "additional_item_shipping": 2.5
        }
    ],
    "product_color_variants": [
        "Antique Cherry Red",
        "Antique Irish Green"
    ]
}
```
