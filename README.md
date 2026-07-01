# Digi Postcard

Turn any photo into a vintage black-and-white postcard — right in your browser. No app install, no server, no sign-up.

## Features

- **Film look** — Grayscale conversion with lifted blacks, contrast boost, subtle grain, warm tint, and vignette
- **Auto date & location** — Reads EXIF metadata from your photo to stamp the date and reverse-geocode the GPS coordinates into a street address
- **Postcard frame** — Classic diagonal-stripe border with black outline
- **Mobile share** — Native share sheet on iOS/Android (via Web Share API) lets you send the result to any app
- **Runs entirely in-browser** — All processing happens on-device using Canvas API. No uploads, no backend, no tracking
- **Single file** — One `index.html`, zero dependencies (besides a CDN-loaded EXIF parser)

## Try it

**[Open Digi Postcard](https://gnitsua.github.io/digi-postcard/)**

Works on any modern browser. Best on mobile — tap to take a photo or pick from your gallery, then share the result.

## How it works

1. Select or capture a photo
2. EXIF date and GPS location are extracted automatically (editable)
3. The image is scaled to 400px wide, converted to grayscale, and processed with film-style effects
4. A postcard frame is drawn around the result
5. Tap **Share Photo** to send it anywhere (or download on desktop)

## Run locally

```bash
# Any static file server works
python3 -m http.server 8080
open http://localhost:8080
```

For mobile testing on your local network, you'll need HTTPS (required for the Web Share API):

```bash
# Generate a self-signed cert
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 30 -nodes -subj '/CN=localhost'

# Serve with HTTPS
python3 -c "
import ssl, http.server
ctx = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
ctx.load_cert_chain('cert.pem', 'key.pem')
srv = http.server.HTTPServer(('0.0.0.0', 8080), http.server.SimpleHTTPRequestHandler)
srv.socket = ctx.wrap_socket(srv.socket, server_side=True)
srv.serve_forever()
"
```

Then visit `https://<your-ip>:8080` on your phone (same Wi-Fi).

## License

MIT
