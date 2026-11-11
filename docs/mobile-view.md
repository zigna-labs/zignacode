# View on your phone (Tailscale)

From your phone, you can **view** the zignacode dashboard (DAG, cards, diffs) running on your PC — even when you are out. The phone cannot operate anything.

The PC and the phone are connected with [Tailscale](https://tailscale.com), and you open it at an https address. It works on a different Wi-Fi or over mobile data.

> **Connecting over Wi-Fi is not supported.** Plain http on the same Wi-Fi (192.168.x.x and so on) is not encrypted, so anyone on the same Wi-Fi could read the token and what is on the screen. Mobile view works only through Tailscale https.

## 1. Install Tailscale (PC and phone)

1. Install Tailscale on both the PC and the phone.
2. Sign in with the **same account** on both.
3. You are ready when both appear in your Tailscale device list.

## 2. Enable HTTPS certificates (once)

Mobile view opens only over https, so this is required.

1. In a browser, open the Tailscale admin console → **DNS**.
2. Make sure **MagicDNS** is enabled.
3. Under **HTTPS Certificates**, press **Enable**.

This is a setting on your Tailscale account. No settings file on your PC changes.

> The certificate contains your machine name and tailnet name (for example `my-pc.tail1234.ts.net`), and it is recorded in the public Certificate Transparency logs. If your machine name is something you would rather keep private, rename it first.

## 3. Turn it on in zignacode

1. Open **⚙** at the left end of the status bar → **Mobile view**.
2. Turn **On / off** **On**.
3. When you see `Available at https://<machine>.<tailnet>.ts.net/m/` right below it, it is working.
4. Press **Show QR** and scan the QR code with the phone's camera.

The https address is open **only while zignacode is running**. It closes when zignacode quits. No persistent Tailscale setting is created.

Only devices on the same Tailscale account can open it. It is not published to the internet.

## Troubleshooting

The line under On / off in the settings tells you what is wrong.

| Message | What to do |
|---|---|
| Tailscale was not found | Install Tailscale on the PC |
| Tailscale is not connected | Start Tailscale on the PC and sign in |
| Enable HTTPS certificates | Do "2. Enable HTTPS certificates" above |
| Port 443 already has another setting | You configured `tailscale serve` on 443 yourself. zignacode will not overwrite it. Remove that setting |
| Could not open the https address | Check the reason shown after it. Turning On / off off and on again retries |

After enabling certificates or starting Tailscale, turn **On / off off and on again** to retry.

- The first visit can be slow while the certificate is issued.
- Creating a new QR code disconnects phones that opened the previous one (scan it again).
