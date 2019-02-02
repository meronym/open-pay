# open-pay
OpenPay (OP) is an open protocol for sending and receiving digital payments denominated in units that closely approximate the value of a fiat asset in the traditional (national and supranational central banking based) economy, through a decentralized network with no paperwork involved. 

- Server registers for their account with us and we get them to install the mobile app 
    - (required for api access)
    - based on open api communication, so anybody can write wrappers for onboarding users
- Server installs mobile app and walks through the process of setting up the user key (maybe recovery?)
- App tells Server "go to wift.me/login"
    - Server get a welcome message and a QR code. The App on screen has a "Scan QR button". Scans the QR
    - Phone talks to networks and negotiates session keys for the web session to take place
    - Dashboard is now linked to the account on the phone thanks to the QR login
- Server accesses a dashboard that monitors activity (money sent and received) and integrates with custody services (e.g. Trustology)
   - until then, we keep half the custody of the account and check the spending policies before signing our part of the transaction
    - the money sit in a 2-of-2 multisig of (our key, their_key) with our key always online and signing only we can know that (1) they have access to their key and (2) they have access to the email account (that's how you set/change spending policies)
- Server reads the integration guide with minimal description of the 2-3 rest endpoints that do all the work
- Server wants to test the code with some test money
    - our dashboard should make the api token obvious.
    - our dashboard should be able to switch in "Test Mode" on demand - this switches the api token to a test version
    - a test key is generated so Server can keep the same requests, but have them run to a clone testnet of the real world.
    During Test Mode:
    - Server needs to get lots of test money in his account (so the mainnet fork should have updated balances for his account)
    - Server should have easy access to the wallets of three dummy accounts that have in turn lots of money to pay with
    - Server should be able to control the invoicing flow and the transfers from dummy accounts from code without effort
    - A part of this code should make sense on mainnet as well.
- Server makes sure everything works well and switches their key to the "real" money network.
- Client visits online purchase page.
- Server generates an invoice that, once paid, grants access to a previleged section of the website.
- Client sees the "Pay Now" user interface in their browser.
- CLient pays with any crypto (start wirth eth-based tokens and then bitcoin) to a dedicated address
- Server receives confirmation of receipt (based on previously defined risk tolerance=zeroconf vs wait for x confirmations proportional to tx value)
- Server registers the payment receipt + confirmation in their internal db
- Server now knows that the user x has paid, and they will gfet the money in their account
- Server gets the money out from their account

THOUGHTS

- You could use OpenPay to charge your users money via OpenPay for accessing your OpenPay gateway. That's justifiable by the costs you will incur for hosting an instance of a synced Parity Full Node and taking on the liability of maintaining it in sync with the main Ethereum network. There could be an additional pool of OpenPay money that could be used as a community staking pool for people operating through the same gateway/hub.

