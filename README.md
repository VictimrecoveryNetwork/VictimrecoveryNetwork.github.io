<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>ScamReport – Report a Scam</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- GSAP -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>

  <!-- EmailJS -->
  <script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>

  <!-- Firebase -->
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js"></script>
</head>

<body class="bg-slate-100 min-h-screen flex flex-col">

  <!-- Header -->
  <header class="bg-slate-900 text-white">
    <div class="max-w-5xl mx-auto px-4 py-4 flex justify-between items-center">
      <h1 class="text-2xl font-bold">ScamReport</h1>
      <nav class="space-x-4 text-sm">
        <a href="#about" class="hover:underline">About</a>
        <a href="#report" class="hover:underline">Report</a>
        <a href="#help" class="hover:underline">Help</a>
      </nav>
    </div>
  </header>

  <!-- Main -->
  <main class="flex-1">

    <!-- HERO -->
    <section id="about" class="relative text-white overflow-hidden">

      <div id="bgImage"
           class="absolute inset-0 bg-cover bg-center bg-no-repeat scale-110"
           style="background-image: url('images/recovery-bg.jpg'); filter: blur(4px);">
      </div>

      <div class="absolute inset-0 bg-gradient-to-b from-black/70 via-black/60 to-black/80"></div>

      <div class="relative max-w-5xl mx-auto px-4 py-20">
        <h2 class="text-4xl md:text-5xl font-bold mb-4">Report scams. Protect others.</h2>
        <p class="text-slate-200 max-w-xl mb-6">
          Use this form to report online scams, fraud attempts, or suspicious activity.
        </p>
        <button onclick="document.getElementById('report').scrollIntoView({behavior:'smooth'})"
                class="bg-emerald-500 hover:bg-emerald-600 text-white px-6 py-3 rounded-lg font-semibold">
          Report a Scam
        </button>
      </div>

    </section>

    <!-- Report Form -->
    <section id="report" class="py-12">
      <div class="max-w-3xl mx-auto px-4 bg-white shadow-md rounded-2xl p-8">
        <h3 class="text-2xl font-bold mb-2">Scam Report Form</h3>
        <p class="text-sm text-slate-500 mb-6">Do not include passwords or sensitive financial information.</p>

        <form id="scamForm" class="space-y-5">

          <input id="name" type="text" placeholder="Your Name (optional)"
                 class="w-full border rounded-lg px-3 py-2">

          <input id="email" type="email" placeholder="Email (optional)"
                 class="w-full border rounded-lg px-3 py-2">

          <select id="type" required class="w-full border rounded-lg px-3 py-2">
            <option value="">Select a type</option>
            <option>Investment / Crypto</option>
            <option>Romance / Dating</option>
            <option>Tech Support</option>
            <option>Phishing Email / SMS</option>
            <option>Online Purchase / Marketplace</option>
            <option>Impersonation (bank, gov, etc.)</option>
            <option>Other</option>
          </select>

          <input id="channel" type="text" placeholder="Where did it happen?"
                 class="w-full border rounded-lg px-3 py-2">

          <input id="date" type="date" class="w-full border rounded-lg px-3 py-2">

          <input id="amount" type="number" placeholder="Amount involved (optional)"
                 class="w-full border rounded-lg px-3 py-2">

          <textarea id="details" required rows="5"
                    class="w-full border rounded-lg px-3 py-2"
                    placeholder="Describe what happened..."></textarea>

          <button type="submit"
                  class="w-full bg-emerald-500 hover:bg-emerald-600 text-white py-3 rounded-lg font-semibold">
            Submit Report
          </button>

        </form>
      </div>
    </section>

  </main>

  <!-- Footer -->
  <footer class="bg-slate-950 text-slate-400 text-xs py-4">
    <div class="max-w-5xl mx-auto px-4 flex justify-between items-center">
      <p>© <span id="year"></span> ScamReport</p>
      <a href="#about" class="hover:underline">Back to top</a>
    </div>
  </footer>

  <!-- JS -->
  <script>
    document.getElementById('year').textContent = new Date().getFullYear();

    gsap.registerPlugin(ScrollTrigger);

    gsap.from("#bgImage", { opacity: 0, duration: 1.5, ease: "power2.out" });

    gsap.to("#bgImage", {
      y: 120, scale: 1.2, ease: "none",
      scrollTrigger: { trigger: "#about", start: "top top", end: "bottom top", scrub: true }
    });

    /* ---------------------------
       FIREBASE INITIALIZATION
    ---------------------------- */
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_AUTH_DOMAIN",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_BUCKET",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    /* ---------------------------
       EMAILJS INITIALIZATION
    ---------------------------- */
    emailjs.init("YOUR_PUBLIC_KEY");

    /* ---------------------------
       FORM SUBMIT HANDLER
    ---------------------------- */
    document.getElementById("scamForm").addEventListener("submit", async (e) => {
      e.preventDefault();

      const data = {
        name: document.getElementById("name").value,
        email: document.getElementById("email").value,
        type: document.getElementById("type").value,
        channel: document.getElementById("channel").value,
        date: document.getElementById("date").value,
        amount: document.getElementById("amount").value,
        details: document.getElementById("details").value,
        timestamp: new Date()
      };

      // Save to Firestore
      await db.collection("reports").add(data);

      // Send Email Notification
      emailjs.send("YOUR_SERVICE_ID", "YOUR_TEMPLATE_ID", data);

      // Generate reference ID
      const ref = "SR-" + Math.floor(100000 + Math.random() * 900000);
      sessionStorage.setItem("scamReportRef", ref);

      // Redirect
      window.location.href = "thank-you.html";
    });
  </script>

</body>
</html>
