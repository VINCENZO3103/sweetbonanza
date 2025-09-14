const puppeteer = require('puppeteer');

const TARGET_URL = 'https://casinoradar.it/sweet-bonanza-candyland-statistiche'; // <-- metti qui la tua URL
const RELOAD_MINUTES = 240; // ricarica ogni 4 ore per sicurezza

(async () => {
  const browser = await puppeteer.launch({
    headless: 'new',
    args: [
      '--no-sandbox',
      '--disable-setuid-sandbox',
      '--disable-dev-shm-usage',
      '--disable-gpu'
    ]
  });

  const page = await browser.newPage();
  page.setDefaultNavigationTimeout(120000);

  // (opzionale) user-agent "normale"
  await page.setUserAgent(
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 ' +
    '(KHTML, like Gecko) Chrome/118.0 Safari/537.36'
  );

  page.on('console', msg => console.log('[page]', msg.type(), msg.text()));
  page.on('pageerror', err => console.error('[pageerror]', err));

  const goto = async () => {
    console.log('Opening page:', TARGET_URL);
    try {
      await page.goto(TARGET_URL, { waitUntil: 'networkidle2' });
      console.log('Page loaded. Collector active.');
    } catch (e) {
      console.error('Goto error:', e.message);
    }
  };

  await goto();

  // ricarico periodico per evitare eventuali leak/reconnect
  setInterval(goto, RELOAD_MINUTES * 60 * 1000);

  // heartbeat per tenere vivo il log del job
  setInterval(() => console.log('heartbeat', new Date().toISOString()), 60 * 1000);
})();
