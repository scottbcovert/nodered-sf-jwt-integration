# Node-RED Salesforce API Integration via OAuth2 JWT Bearer Flow

This serves as a PoC to demonstrate how Node-RED can be setup to integrate with the Salesforce REST API via the [OAuth2 JWT bearer flow](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_jwt_flow.htm&type=5)

## Pre-Requisites
1. npm
2. Docker
3. Docker Compose CLI

## Getting Started

1. Clone repo
2. Run `npm install`
3. [Create a connected app](https://help.salesforce.com/articleView?id=connected_app_create.htm&type=5) in your Salesforce org
4. Select `Enable OAuth Settings` and set the OAuth scope for your connected app to include `full`, `refresh_token`, & `offline_access`
5. The callback URL is not used during the OAuth2 JWT flow, but it's required so you can just set it to `http://localhost:8080/callback`
6. [Create a private key and self-signed digital certificate](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_auth_key_and_cert.htm) - take note that this guide creates a cert that will expire in one year, you may wish to change the `-days` parameter for a longer or shorter lived certificate
7. On your connected app settings, select `Use digital signatures` and then upload the `server.crt` file created previously
8. Spin up the Node-RED server locally on by executing `docker-compose up --build`
9. Node-RED should be available by visiting `localhost:1880`
10. Note the `setConfig` node that injects the required settings to generate the JWT
11. Set `CLIENT_ID` to your connected app's consumer key
12. To integrate with a Salesforce sandbox change the `LOGIN_URL` value to `https://test.salesforce.com`
13. Set `USERNAME` to that of the user you want to authenticate as. This user will need to first run through the OAuth2 web flow a *single* time by visiting LOGIN_URL/services/oauth2/authorize?response_type=token&client_id=CLIENT_ID&redirect_uri=LOGIN_URL/services/oauth2/success - note you'll need to replace `LOGIN_URL` and `CLIENT_ID` in that URL
14. Set `PRIVATE_KEY` to that of the private key matching the self-generated SSL cert associated with your connnected app. Note you will need to wrap the key in `"` and use `\n` characters as opposed to literal line breaks similar to the example value
15. Click `Deploy` in Node-RED to save and deploy any and all changes
16. Verify your Salesforce auth info and user list being logged to the debug window

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details