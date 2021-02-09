# conjur-storing-binary-files

This repo contains the steps for 

1. Setup Conjur OSS by following the quick start guide to "Setup a Conjur OSS environment" & "Define Policy" according to offical doc (https://www.conjur.org/get-started/)

2. Create a Keystore file by `keytool -genkeypair -alias domain -keyalg RSA -keystore keystore.jks` 

3. (optional) Verify the generated keystore file by `keytool -list -keystore keystore.jks`
   
4. Save the file to Conjur
```
docker cp keystore.jks conjur_client:/tmp
docker-compose exec client sh -c "cat /tmp/keystore.jks | conjur variable values add BotApp/secretVar"
```

5. Get the secret
```
docker-compose exec client sh -c "conjur variable value BotApp/secretVar > /tmp/output.jks"
docker cp conjur_client:/tmp/output.jks .
```

6. (optional) Show the raw REST API `docker-compose exec client sh -c "RESTCLIENT_LOG=stdout conjur variable value BotApp/secretVar"`

7. Verify the generated keystore file
   Try `keytool -list -keystore output.jks`
