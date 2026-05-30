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
    - [Webhooks](#webhooks)
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

#### Description
This endpoint allows creating new orders with different order types and print methods.

- `order_type` determines whether the order requires label creation or already includes it.
  - `normal`: Full order details required, used when there is no shipping label and needs to be **purchase shipping labels**.
  - `label`: Only minimal customer info (may be masked with `*`), **tracking number** and **label link** must be provided.

- `order_page_type` determines the print method or product category:
  - Leave empty or omit → defaults to **DTF**.
  - Set to `"DTG"` for **DTG printing** orders.
  - Set to `"NECKLACE"` for **Necklace** orders.

- `quantity`, `size`, `color`, and `shirt_type` are **always required**.
- `front_design`, `back_design`, and `extra_design_*` are **optional**,  
  but if included → their corresponding mockup fields are **required**.

#### Field Definitions

##### Order-Level Fields

| Field | Type | Required | Description | Condition / Notes |
|--------|------|-----------|--------------|------------------|
| `order_unique_code` | string | Yes | Unique identifier for internal tracking | Must be unique per order |
| `external_id` | string | Yes | External platform order ID | Usually from marketplace (e.g. TikTok, Shopify) |
| `order_type` | string (`normal` \| `label`) | Yes | Defines order source type | See description above |
| `order_page_type` | string (`DTF` \| `DTG` \| `NECKLACE`) | No | Printing technology / Category | Default = `DTF` |
| `customer_name` | string | Optional | Customer full name | Required for `normal` orders; can be masked for `label` |
| `customer_email` | string | Optional | Customer email | Required for `normal`; can be masked for `label` |
| `customer_contact` | string | Optional | Customer phone | Required for `normal`; optional for `label` |
| `delivery_street` | string | Optional | Street address | Required for `normal`; can be masked for `label` |
| `delivery_unit` | string | No | Apartment / unit / suite |  |
| `delivery_city` | string | Optional | City name | Required for `normal`; can be masked for `label` |
| `delivery_state` | string | Optional | State / province | Required for `normal`; can be masked for `label` |
| `delivery_zip_code` | string | Optional | Postal code | Required for `normal`; can be masked for `label` |
| `delivery_country` | string | Optional | Country (ISO code or name) | Required for `normal`; can be masked for `label` |
| `tracking_number` | string | Optional | Shipment tracking number | Required for `label`; blank for `normal` |
| `delivery_label` | string (URL) | Optional | Shipping label file (PDF) | Required for `label`; blank for `normal` |
| `items` | array | Yes | List of products in the order | Must contain at least one item |

---

##### Item-Level Fields

| Field | Type | Required | Description | Condition / Notes |
|--------|------|-----------|--------------|------------------|
| `quantity` | integer | Yes | Quantity of this item | Always required |
| `note` | string | No | Additional note per item |  |
| `size` | string | Yes | Product size (e.g., `M`, `L`, `2XL`) | Always required |
| `color` | string | Yes | Product color (e.g., `BLACK`, `SAND`, `black box`, `light wood box`) | Always required. For NECKLACE: `black box` or `light wood box`. |
| `shirt_type` | string | Yes | Product type (e.g., `tshirt`, `br003-silver`) | Always required. See the available options listed below. |
| `front_design` | string (URL) | Optional | Front design file | If provided → `mockup_front` required |
| `back_design` | string (URL) | Optional | Back design file | If provided → `mockup_back` required |
| `mockup_front` | string (URL) | Optional | Front mockup image | Required if `front_design` is provided |
| `mockup_back` | string (URL) | Optional | Back mockup image | Required if `back_design` is provided |
| `extra_design_1` | string (URL) | Optional | Optional extra design #1 | If provided → `mockup_extra_1` required |
| `mockup_extra_1` | string (URL) | Optional | Optional extra mockup #1 | Required if `extra_design_1` is provided |
| `extra_design_2` | string (URL) | Optional | Optional extra design #2 | If provided → `mockup_extra_2` required |
| `mockup_extra_2` | string (URL) | Optional | Optional extra mockup #2 | Required if `extra_design_2` is provided |
| `extra_design_3` | string (URL) | Optional | Optional extra design #3 | If provided → `mockup_extra_3` required |
| `mockup_extra_3` | string (URL) | Optional | Optional extra mockup #3 | Required if `extra_design_3` is provided |
| `is_rush` | boolean | No | Rush production flag |  |

---

##### Available `shirt_type` Options

| `shirt_type` | Description |
|---------------|-------------|
| `fulltshirt` | All-over print T-shirt |
| `tshirt` | Standard unisex T-shirt |
| `ytshirt` | Youth T-shirt same as the Kid’s T-shirt but with a different naming convention (use `kidshirt` instead) |
| `kidshirt` | Kid’s T-shirt (**recommended**) |
| `hoodie` | Hoodie with hood |
| `sweatshirt` | Sweatshirt without hood |
| `comfort` | Comfort Colors heavy cotton T-shirt |
| `tanktop` | Sleeveless Tank Top |
| `bella` | Bella Canvas T-shirt |

##### Available `shirt_type` Options for NECKLACE

| Series | `shirt_type` Values |
|--------|----------------------|
| **ALN series** | `aln003` |
| **BR series** | `br001`, `br002`, `br003-silver`, `br005-silver`, `br006`, `br007` |
| **BSCN series** | `bscn-cn01`, `bscn005` |
| **CBN series** | `cbnln012`, `cbndn013`, `cbnfl014` |
| **CN series** | `cn007`, `cn008` |
| **CWN series** | `cwn011` |
| **DN series** | `dn002`, `dne010` |
| **FLE / FLN series** | `fle008`, `fln006`, `gold_fln006` |
| **LKE / LKN series** | `lke009`, `lkn001`, `gold_lkn001` |
| **ILN series** | `iln004` |


#### Request - Order Type `normal`
```json
{
    "orders": [
        {
            "order_unique_code": "AZBV_TT0811_007_577036687762100371",
            "external_id": "577036687762100371",
            "order_type": "normal",
            "order_page_type": "",
            "customer_name": "Maya Smith",
            "customer_email": "v4bD2USHHNJ5RYNGGVP4MQ6C33W24@scs.tiktokw.us",
            "customer_contact": "(+1)3212793ABC",
            "delivery_street": "550 Battery St",
            "delivery_unit": "Apt 123",
            "delivery_city": "San Francisco",
            "delivery_state": "California",
            "delivery_zip_code": "94111",
            "delivery_country": "US",
            "tracking_number": "",
            "delivery_label": "",
            "items": [
                {
                    "quantity": 1,
                    "note": "",
                    "size": "2XL",
                    "color": "SAND",
                    "shirt_type": "tshirt",
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
            ] 
        }
    ]
}
```

#### Request - Order Type `label`
```json
{
    "orders": [
        {
            "order_unique_code": "AZBV_TT0811_007_577036687762100371",
            "external_id": "577036687762100371",
            "order_type": "label",
            "order_page_type": "",
            "customer_name": "S***nna M***ne",
            "customer_email": "v4bD2USHHNJ5RYNGGVP4MQ6C33W24@scs.tiktokw.us",
            "customer_contact": "(+1)501*****46",
            "delivery_street": "42************",
            "delivery_unit": "",
            "delivery_city": "N ***********",
            "delivery_state": "Arkansas",
            "delivery_zip_code": "72***",
            "delivery_country": "US",
            "tracking_number": "9400109208316578510797",
            "delivery_label": "https://open-fs.tiktokshops.us/wsos_v2/oec_fulfillment_doc_tts/object/wsos6875cd8983598b37?expire=1752637210&skipCookie=true&timeStamp=1752550810&sign=4441f3e1b06a2f3f06aa85d9dd94051f36d2170a4b88778239e872fd0a2d8dd2",
            "items": [
                {
                    "quantity": 1,
                    "note": "",
                    "size": "2XL",
                    "color": "SAND",
                    "shirt_type": "tshirt",
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
            ] 
        }
    ]
}
```

#### Request - Order Page Type `DTG`
```json
{
    "orders": [
        {
            "order_unique_code": "AZBV_TT0811_007_577036687762100371",
            "external_id": "577036687762100371",
            "order_type": "label",
            "order_page_type": "DTG",
            "customer_name": "S***nna M***ne",
            "customer_email": "v4bD2USHHNJ5RYNGGVP4MQ6C33W24@scs.tiktokw.us",
            "customer_contact": "(+1)501*****46",
            "delivery_street": "42************",
            "delivery_unit": "",
            "delivery_city": "N ***********",
            "delivery_state": "Arkansas",
            "delivery_zip_code": "72***",
            "delivery_country": "US",
            "tracking_number": "",
            "delivery_label": "",
            "items": [
                {
                    "quantity": 1,
                    "note": "",
                    "size": "2XL",
                    "color": "SAND",
                    "shirt_type": "tshirt",
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
            ] 
        }
    ]
}
```

#### Request - Order Page Type `NECKLACE`
```json
{
  "orders": [
    {
      "order_unique_code": "AZBV_TT0811_007_577036687762100371",
      "external_id": "577036687762100371",
      "order_type": "label",
      "customer_name": "S***nna M***ne",
      "customer_email": "v4bD2USHHNJ5RYNGGVP4MQ6C33W24@scs.tiktokw.us",
      "customer_contact": "(+1)501*****46",
      "delivery_street": "42************",
      "delivery_unit": "",
      "delivery_city": "N ***********",
      "delivery_state": "Arkansas",
      "delivery_zip_code": "72***",
      "delivery_country": "US",
      "tracking_number": "9400109208316578510797",
      "delivery_label": "https://open-fs.tiktokshops.us/wsos_v2/oec_fulfillment_doc_tts/object/wsos6875cd8983598b37?expire=1752637210&skipCookie=true&timeStamp=1752550810&sign=4441f3e1b06a2f3f06aa85d9dd94051f36d2170a4b88778239e872fd0a2d8dd2",
      "order_page_type": "NECKLACE",
      "items": [
        {
          "quantity": 1,
          "color": "light wood box",
          "shirt_type": "br003-silver",
          "extra_design_1": "lh3.googleusercontent.com/d/1HTVc06ODr7ErCH48RDzbJrZM9DykyNll=s0",
          "is_rush": false
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
`/api/orders?search=23-13452-90981`
</br>
Multiple Search
</br>
`/api/orders?search=23-13452-90981 21-13455-64028`
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

---

### Webhooks

Webhooks allow you to receive real-time notifications when certain events occur in Printiz.

#### Setup via UI
The easiest way to configure your webhook is through the Printiz Dashboard:

1. Log in to your **Printiz Dashboard**.
2. Navigate to the **Settings** page from the sidebar.
3. Click on the **Webhook** tab.
4. Enter your **Webhook URL** (e.g., `https://your-domain.com/webhook`).
5. Enter a **Webhook Secret Key** (optional but recommended for security).
6. Click **Save Webhook Configuration**.
7. (Optional) Use the **Test Webhook** button to send a sample payload to your URL and verify the connection.

#### Configuration via API
You can also configure your webhook URL and secret key via the API.

**PUT** `/api/users/webhook/config`

##### Request
```json
{
  "webhook_url": "https://your-domain.com/webhook",
  "webhook_secret_key": "your_secure_secret_key"
}
```
##### Response
```json
{
    "success": true, 
    "message": "Webhook configuration updated successfully",
    "data": {
        "webhook_url": "https://your-domain.com/webhook",
        "webhook_secret_key": "your_secure_secret_key",
        "configured_at": "2026-05-30T10:00:00Z"
    }
}
```

#### Signature Verification
To ensure that a webhook was sent by Printiz, you should verify the signature included in the `X-Webhook-Signature` header. 

The signature is a **HMAC SHA256** hex digest of the raw JSON payload using your `webhook_secret_key` as the key.

Example verification (Node.js):
```javascript
const crypto = require('crypto');

const signature = request.headers['x-webhook-signature'];
const payload = JSON.stringify(request.body);
const expectedSignature = crypto
  .createHmac('sha256', WEBHOOK_SECRET_KEY)
  .update(payload)
  .digest('hex');

if (signature === expectedSignature) {
  // Valid request
}
```

#### Events

##### `order.status_changed`
Triggered whenever an order's status is updated (e.g., from `Order_Await_Approval` to `Order_In_Production`).

###### Payload
```json
{
    "event": "order.status_changed",
    "timestamp": 1758443070,
    "data": [
        {
            "order_id": 81,
            "order_unique_code": "13-13468-53887",
            "updated_status": "Order_Await_Approval"
        }
    ]
}
```

#### Testing
You can trigger a test webhook to verify your endpoint is correctly receiving and validating requests.

**POST** `/api/users/webhook/test`

##### Request
```json
{
  "webhook_url": "https://your-domain.com/webhook",
  "webhook_secret_key": "your_secure_secret_key"
}
```
##### Response
```json
{
    "success": true,
    "message": "Webhook test successful! Received status 200",
    "status_code": 200
}
```
