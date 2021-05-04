Instead of manually uploading the artifact with the WebUI, you can also upload your artifact programmatically.
The option is also useful if you want to automate this step.

First, you need to create an API Token as our REST API is secured.

Back in FrontLine, click on the {{< icon key >}} icon on the Sidenav (navigation bar on the left of the screen).

{{< img src="api_token_navigation.png" alt="Sidenav navigation" >}}

Click on the Create button in order to create an API Token in FrontLine.
Make sure to grant either `Artifacts` or `All` permissions.
Choose the name you want, then click Save.

{{< img src="api_token_create_1.png" alt="Create an API Token" >}}

Make sure to save the API Token value somewhere, FrontLine will not be able to provide you with a token's value later.

{{< img src="api_token_create_2.png" alt="Save an API Token value" >}}

Now, go to the artifact repository you've created previously and copy its id.

{{< img src="artifact_copy_id.png" alt="Copy an Artifact's id" >}}


You can now upload your artifact, eg with `curl`:

```
curl -X PUT --upload-file <ARTIFACT_LOCAL_PATH> \
  "https://<DOMAIN>/api/public/artifacts/<ARTIFACT_ID>/content?filename=<ARTIFACT_FILE_NAME>" \
  -H "Authorization:<API_TOKEN>"
```

For example:

```
curl -X PUT --upload-file {param} \
  "https://yourorg.beta.gatling.io/api/public/artifacts/8ba9511a-0376-409e-a4f3-a8d8f85624c7/content?filename=artifact.jar" \
  -H "Authorization:jAsAFAWes-Lif6BaMYN0JngAUCzdRw_dS1oUodYUvsdi8JwE0m3OzF_SV1rwkND8q"
```
