# 🤖 streaming datafication lab

- [notebook ipynb](https://colab.research.google.com/github/mab253/bigdata_spring26/blob/main/week13/polymarket_class(3).ipynb) -  Google Colab ![colab_logo_32px](https://github.com/mab253/dataviz_fall23/assets/17707843/9f26ae0a-cf0f-42c2-a1f5-584bb38a36c7)

## error correction!

you can replace the websocket cell code with the following:

```
###### import websocket
import threading
import time
from IPython.display import display, clear_output
import websocket
import ssl

token_ids = []
for m in markets_raw:
    try:
        tokens = json.loads(m.get("clobTokenIds", "[]"))
        if tokens:
            token_ids.append(tokens[0])
    except:
        pass

token_ids = token_ids[:5]
print(f"Got {len(token_ids)} token IDs: {token_ids}")
token_ids = [t for t in token_ids if t != '?'][:5]

stream_events = []
STREAM_SECONDS = 60  # auto-stop after this long

def on_open(ws):
    print(f"🟢 Connected! Subscribing to {len(token_ids)} markets...")
    sub = {"assets_ids": token_ids, "type": "Market"}
    ws.send(json.dumps(sub))

def on_message(ws, message):
    try:
        events = json.loads(message)
        if not isinstance(events, list):
            events = [events]
        for event in events:
            event_type = event.get('event_type', event.get('type', 'unknown'))
            asset_id   = event.get('asset_id', event.get('market', ''))[:12] + '...'
            price      = event.get('price', event.get('last_trade_price', None))
            stream_events.append({
                'type':     event_type,
                'asset':    asset_id,
                'price':    float(price) if price else None,
                'raw':      str(event)[:80]
            })
        clear_output(wait=True)
        sdf = pd.DataFrame(stream_events[-20:])  # show last 20 events
        print(f"🔴 LIVE STREAM — {len(stream_events)} events received so far")
        print(f"   Streaming for up to {STREAM_SECONDS}s. Stop button to end early.\n")
        display(sdf)
    except Exception as e:
        pass

def on_error(ws, error):
    print(f"⚠️ WebSocket error: {error}")

def on_close(ws, close_status_code, close_msg):
    print(f"\n🔴 Stream closed. Total events received: {len(stream_events)}")

ws = websocket.WebSocketApp(
    "wss://ws-subscriptions-clob.polymarket.com/ws/market",
    on_open=on_open,
    on_message=on_message,
    on_error=on_error,
    on_close=on_close
)

# Run in background thread, auto-close after STREAM_SECONDS
import ssl
ssl_ctx = ssl.create_default_context()
ssl_ctx.check_hostname = False
ssl_ctx.verify_mode = ssl.CERT_NONE

t = threading.Thread(target=lambda: ws.run_forever(sslopt={"context": ssl_ctx}))

t.daemon = True
t.start()

time.sleep(STREAM_SECONDS)
ws.close()
print("\n✅ Stream ended.")
```

