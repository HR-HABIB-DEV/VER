# Deploy Cobalt API on Render

This repository is configured to deploy the **Cobalt API** as a Render Docker web
service. The upstream web frontend is not hosted by this service.

## Deploy

1. Push this repository to your own GitHub repository.
2. In Render, choose **New + → Blueprint** and select that GitHub repository.
   Alternatively create a **Web Service**, choose Docker, and use the repository
   root as the Docker context.
3. Render reads `render.yaml` and builds the root `Dockerfile`.
4. After Render creates the service, open **Environment** and set `API_URL` to
   the public URL assigned to this service, including `https://`, for example
   `https://cobalt-api-example.onrender.com`.
5. Save and redeploy.

`API_URL` is required by Cobalt. Do not put a trailing path such as `/api` in it.
The service uses Render's injected `PORT` automatically; `API_PORT` can be left
unset. If you manually set `API_PORT`, it takes precedence over `PORT` and must
match the port Render expects.

## Start / health check

The Dockerfile starts the API with `node src/cobalt`. `GET /` returns Cobalt's
server information and is used as the Render health check.

## Notes

- This deploys the API only. It does not create a separate hosted web frontend.
  Configure a Cobalt-compatible frontend to use this API URL.
- No `COBALT_API_KEY` or `COBALT_API_URL` is required for this upstream Cobalt
  API deployment. `API_URL` is the required public base URL of this instance.
- This is a public API by default. For a personal/private instance, review
  Cobalt's official API authentication and security documentation before
  enabling access controls.
- Render free instances may sleep when idle and cold-start on the next request.
- Use the software only for content you are authorized to download.
