---
name: Search hotels and create a booking
description: Find a hotel, read its rate plans, and create a booking with the Impala Hotel Booking API.
api: openapi/impala-hotels-openapi-original.json
operations: [listHotels, retrieveHotel, listRatePlansForHotel, listRatePlanForHotelForRatePlanId, createBooking]
---

# Search hotels and create a booking

> Historical skill. Impala (getimpala.com / impala.travel) is defunct and its
> hosts no longer resolve; grounded in the preserved OpenAPI for reference.

## Auth
- Send every request with an `x-api-key` header (sandbox vs production keys are distinct).
- Confirming a booking requires a payment bearer token (`paymentAuth`, HTTP bearer JWT).
- Base URL: `https://sandbox.impala.travel/v1` (test) or `https://api.impala.travel/v1` (live).

## Steps
1. **List hotels** — call `listHotels` (`GET /hotels`). Page with `size`/`offset`; follow the `next` URL in the returned `pagination` object.
2. **Retrieve a hotel** — call `retrieveHotel` (`GET /hotels/{hotelId}`) for full detail (room types, amenities, currency, check-in/out).
3. **Read rate plans** — call `listRatePlansForHotel` (`GET /hotels/{hotelId}/rate-plans`) for the rate calendar, or `listRatePlanForHotelForRatePlanId` (`GET /hotels/{hotelId}/rate-plans/{ratePlanId}`) for one plan. (Both are Beta endpoints.)
4. **Create the booking** — call `createBooking` (`POST /bookings`) with the guest contact, dates, and selected room/rate.

## Rules
- Errors return a `genericError` object (`code`, `message`) — not RFC 9457. Do not surface `message` to guests; it may contain sensitive detail.
- A `Warning` response header means a 2xx was only partially processed — inspect it.
- On a 500, capture the `x-correlation-id` response header for support.
- There is no idempotency key; do not blindly retry `createBooking` on a network error — reconcile with `listBookings` first.
