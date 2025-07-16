## Overview

The Paydock Mobile SDK provides pre-built UI widgets that seamlessly integrate core Paydock functionality directly into your native iOS or Android applications.

Once you've added and initialised the Mobile SDK in your application, you can leverage these widgets to implement various payment flows and features, including:

*   **Digital Wallets:** Google Pay, Apple Pay
*   **Payment Methods:** PayPal, Afterpay, Coles Pay, Card Details, Gift Cards
*   **Processing:** Securely handle 3DS challenges (both integrated and standalone)
*   **Other Features:** PayPal vault (card saving), Address Capture

## Getting Started

Follow these steps to integrate and use the Mobile SDK:

1.  **[Add the SDK](./setup/installation.md)** as a dependency to your iOS or Android project.
2.  **[Initialise the SDK](./setup/initialise.md)** by providing necessary configuration, such as your `environment` and public key.
3.  **(Optional) [Customise UI Appearance](./theming/theming.md):** Tailor the look and feel of the SDK widgets to match your app's design using our theming guide.
4.  **Implement Payment Features:**
    *   **Digital Wallets:**
        *   [Google Pay Guide](./digital-wallet-widgets/googlepay.md)
        *   [Apple Pay Guide](./digital-wallet-widgets/applepay.md)
    *   **Payment Methods :**
        *   [PayPal Guide](./digital-wallet-widgets/paypal.md)
        *   [PayPal Vault Guide](./digital-wallet-widgets/paypalvault.md)
        *   [Afterpay Guide](./digital-wallet-widgets/afterpay.md)
        *   [Click To Pay Guide](./widgets/clicktopay.md)
        *   [Coles Pay Guide](./digital-wallet-widgets/colespay.md)
        *   [Gift Card Widget Guide](./widgets/giftcard.md)
        *   [Card Details Widget Guide](./widgets/card.md)
    *   **Processing Methods :**
        *   [Integrated 3DS Guide](./widgets/integrated3ds.md)
        *   [Standalone 3DS Guide](./widgets/standalone3ds.md)
    *   **Other Features :**
        *   [PayPal Vault Guide](./digital-wallet-widgets/paypalvault.md)
        *   [Address Capture Widget Guide](./widgets/address.md)
