# Usage Plan

In Amazon API Gateway, a Usage Plan is essentially a "service contract" you offer to your API consumers. It allows you to control who can access your API, how fast they can call it, and how many total calls they can make over a period of time.

If you are building a SaaS product or a public API, usage plans are how you create "Bronze, Silver, and Gold" tiers for your customers.

***

### 1. Key Components of a Usage Plan

To make a usage plan work, you need three interconnected pieces:

* API Key: A unique string (the "ID card") given to a client.
* Usage Plan: The "rules" (throttling and quotas).
* API Stage: The actual API deployment (e.g., `prod` or `v1`) that the plan applies to.

***

### 2. The Three Main Controls

#### A. Throttling (The Speed Limit)

This prevents a single user from overwhelming your backend at any given moment. It uses a Token Bucket algorithm:

* Rate: The steady-state number of requests per second (RPS) allowed.
* Burst: The maximum number of requests a user can make in a split second (to handle tiny spikes) before they get a `429 Too Many Requests` error.

#### B. Quota (The Data Cap)

This is for long-term management (billing or resource preservation).

* Limit: The total number of requests allowed per Day, Week, or Month.
* Example: A "Free Tier" might have a quota of 1,000 requests per month.

#### C. API Stages

A usage plan isn't global; you link it to specific APIs and Stages. This allows you to have a "Testing Plan" for your `dev` stage and a "Premium Plan" for your `prod` stage.

***

### 3. How Throttling Priority Works

AWS checks limits in a specific order. If any of these are hit, the request is throttled:

1. Usage Plan Level: Limits set for a specific API Key.
2. Method Level: Limits set for a specific endpoint (e.g., `GET /users`).
3. Account Level: The overall limit for your entire AWS account in that region (default is usually 10,000 RPS).

***

### 4. Setup Workflow (Step-by-Step)

If you wanted to set up a "Silver Plan" today, here is the flow:

1. Requirement: Your API methods must have "API Key Required" set to `true` in the Method Request settings.
2. Create Usage Plan: Go to API Gateway -> Usage Plans -> Create. Set your Rate (e.g., 50), Burst (e.g., 100), and Quota (e.g., 5,000 per month).
3. Associate Stage: Add your API and the specific Stage (e.g., `prod`) to this plan.
4. Create/Add API Key: Create a new API Key for your customer and Add to Usage Plan.
5. Client Usage: The customer must now include the header `x-api-key: YOUR_KEY_HERE` in every request.

***

#### Comparison: Usage Plans vs. Standard Throttling

| **Feature**    | **Standard (Stage) Throttling**    | **Usage Plans**                  |
| -------------- | ---------------------------------- | -------------------------------- |
| Identification | Anonymous (applies to all traffic) | Identified via API Key           |
| Tiers          | One size fits all                  | Multiple (Basic, Pro, etc.)      |
| Quotas         | Not available                      | Daily/Weekly/Monthly caps        |
| Best For       | Basic protection of backends       | Monetization and client-tracking |

