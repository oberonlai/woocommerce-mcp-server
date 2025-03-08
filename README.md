# WooCommerce MCP Server

A Model Context Protocol (MCP) server for WooCommerce integration, compatible with Windows, macOS, and Linux.

## Overview

This MCP server enables interaction with WooCommerce stores through the WordPress REST API. It provides comprehensive tools for managing all aspects of products, orders, customers, shipping, taxes, discounts, and store configuration using JSON-RPC 2.0 protocol.

## Installation

1. Clone the repository
2. Install dependencies:
```bash
npm install
```
3. Build the project:
```bash
npm run build
```

## Configuration

Add the server to your MCP settings file with environment variables for WooCommerce credentials:

```json
{
  "mcpServers": {
    "woocommerce": {
      "command": "node",
      "args": ["path/to/build/index.js"],
      "env": {
        "WORDPRESS_SITE_URL": "https://your-wordpress-site.com",
        "WOOCOMMERCE_CONSUMER_KEY": "your-woocommerce-consumer-key",
        "WOOCOMMERCE_CONSUMER_SECRET": "your-woocommerce-consumer-secret"
      }
    }
  }
}
```

The environment variables are:
- WORDPRESS_SITE_URL: Your WordPress site URL (WooCommerce is a WordPress plugin)
- WOOCOMMERCE_CONSUMER_KEY: WooCommerce REST API consumer key
- WOOCOMMERCE_CONSUMER_SECRET: WooCommerce REST API consumer secret

You can also provide these credentials in the request parameters if you prefer not to use environment variables.

## Available Methods

### WooCommerce Methods

#### WooCommerce Product Meta Data

##### get_product_meta
Retrieves metadata for a WooCommerce product.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- productId: ID of the product to get metadata for
- metaKey: (optional) Specific meta key to retrieve

##### create_product_meta
Creates or updates metadata for a WooCommerce product.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- productId: ID of the product to add metadata to
- metaKey: Meta key to create/update
- metaValue: Value to associate with the key

##### update_product_meta
Alias for create_product_meta that works for both creating new and updating existing metadata.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- productId: ID of the product to update metadata for
- metaKey: Meta key to update
- metaValue: New value for the metadata

##### delete_product_meta
Deletes a metadata entry from a WooCommerce product.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- productId: ID of the product to remove metadata from
- metaKey: Meta key to delete

#### WooCommerce Order Meta Data

##### get_order_meta
Retrieves metadata for a WooCommerce order.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- orderId: ID of the order to get metadata for
- metaKey: (optional) Specific meta key to retrieve

##### create_order_meta
Creates or updates metadata for a WooCommerce order.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- orderId: ID of the order to add metadata to
- metaKey: Meta key to create/update
- metaValue: Value to associate with the key

##### update_order_meta
Alias for create_order_meta that works for both creating new and updating existing metadata.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- orderId: ID of the order to update metadata for
- metaKey: Meta key to update
- metaValue: New value for the metadata

##### delete_order_meta
Deletes a metadata entry from a WooCommerce order.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- orderId: ID of the order to remove metadata from
- metaKey: Meta key to delete

#### WooCommerce Customer Meta Data

##### get_customer_meta
Retrieves metadata for a WooCommerce customer.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- customerId: ID of the customer to get metadata for
- metaKey: (optional) Specific meta key to retrieve

##### create_customer_meta
Creates or updates metadata for a WooCommerce customer.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- customerId: ID of the customer to add metadata to
- metaKey: Meta key to create/update
- metaValue: Value to associate with the key

##### update_customer_meta
Alias for create_customer_meta that works for both creating new and updating existing metadata.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- customerId: ID of the customer to update metadata for
- metaKey: Meta key to update
- metaValue: New value for the metadata

##### delete_customer_meta
Deletes a metadata entry from a WooCommerce customer.

Parameters:
- siteUrl: (optional if set in env) WordPress site URL
- consumerKey: (optional if set in env) WooCommerce consumer key
- consumerSecret: (optional if set in env) WooCommerce consumer secret
- customerId: ID of the customer to remove metadata from
- metaKey: Meta key to delete

## Security Note

For WooCommerce REST API access, you need to generate API keys. You can create them in your WordPress dashboard under WooCommerce → Settings → Advanced → REST API.

## Example Usage

### WooCommerce API Example:

Using environment variables:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "get_products",
  "params": {
    "perPage": 20,
    "page": 1,
    "filters": {
      "category": 19,
      "status": "publish"
    }
  }
}
```

Create a product:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "create_product",
  "params": {
    "siteUrl": "https://your-wordpress-site.com",
    "consumerKey": "your-consumer-key",
    "consumerSecret": "your-consumer-secret",
    "productData": {
      "name": "Premium T-Shirt",
      "type": "simple",
      "regular_price": "29.99",
      "description": "Comfortable cotton t-shirt, available in various sizes.",
      "short_description": "Premium quality t-shirt.",
      "categories": [
        {
          "id": 19
        }
      ],
      "images": [
        {
          "src": "http://example.com/wp-content/uploads/2022/06/t-shirt.jpg"
        }
      ]
    }
  }
}
```

### WooCommerce Metadata Examples:

Add metadata to a WooCommerce product:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "create_product_meta",
  "params": {
    "productId": 456,
    "metaKey": "_custom_product_field",
    "metaValue": {
      "special_attribute": "value",
      "another_attribute": 42
    }
  }
}
```

Add metadata to a WooCommerce order:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "create_order_meta",
  "params": {
    "orderId": 789,
    "metaKey": "_delivery_instructions",
    "metaValue": "Leave package at the back door"
  }
}
```

Retrieve all metadata for a customer:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "get_customer_meta",
  "params": {
    "customerId": 42
  }
}
```

## Requirements

- Node.js 20.0.0 or higher
- WordPress site with WooCommerce plugin installed
- WooCommerce REST API keys

## License

MIT License - See LICENSE file for details
