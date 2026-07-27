---
name: Retrieve, amend, or cancel a booking
description: Look up an existing Impala booking and change, update its contact, or cancel it.
api: openapi/impala-hotels-openapi-original.json
operations: [listBookings, retrieveBooking, updateBooking, updateBookingContact, cancelBooking]
---

# Retrieve, amend, or cancel a booking

> Historical skill. Impala (getimpala.com / impala.travel) is defunct and its
> hosts no longer resolve; grounded in the preserved OpenAPI for reference.

## Auth
- Send every request with an `x-api-key` header (`https://sandbox.impala.travel/v1` or `https://api.impala.travel/v1`).

## Steps
1. **Find the booking** — call `listBookings` (`GET /bookings`), paging with `size`/`offset` and the `pagination.next` URL, or go straight to `retrieveBooking` (`GET /bookings/{bookingId}`) if you have the id.
2. **Amend the stay** — call `updateBooking` (`PUT /bookings/{bookingId}`) to change dates/rooms.
3. **Update the guest contact** — call `updateBookingContact` (`PUT /bookings/{bookingId}/booking-contact`).
4. **Cancel** — call `cancelBooking` (`DELETE /bookings/{bookingId}`); check the hotel's cancellation policy/fees on the booking first.

## Rules
- Errors return the `genericError` envelope (`code`, `message`); a `Warning` header flags partial processing.
- Quote the `x-correlation-id` response header when reporting 500s to `support@impala.travel`.
- No idempotency key — verify state with `retrieveBooking` before retrying a mutating call.
