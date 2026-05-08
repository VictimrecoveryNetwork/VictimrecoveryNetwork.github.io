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

    <!-- HERO WITH BLUR + PARALLAX + OVERLAY -->
    <section id="about" class="relative text-white overflow-hidden">

      <!-- Background Image -->
      <div id="bgImage"
           class="absolute inset-0 bg-cover bg-center bg-no-repeat scale-110"
           style="background-image: url('images/recovery-bg.jpg'); filter: blur(4px);">
      </div>

      <!-- Dark Gradient Overlay -->
      <div class="absolute inset-0 bg-gradient-to-b from-black/70 via-black/60 to-black/80"></div>

      <!-- Foreground Content -->
      <div class="relative max-w-5xl mx-auto px-4 py-20">
        <h2 class="text-4xl md:text-5xl font-bold mb-4">Report scams. Protect others.</h2>
        <p class="text-slate-200 max-w-xl mb-6">
          Use this form to report online scams, fraud attempts, or suspicious activity. Your report helps protect others.
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

          <div>
            <label class="block text-sm font-medium mb-1">Your Name (optional)</label>
            <input type="text" name="name" class="w-full border rounded-lg px-3 py-2">
          </div>

          <div>
            <label class="block text-sm font-medium mb-1">Email (optional)</label>
            <input type="email" name="email" class="w-full border rounded-lg px-3 py-2">
          </div>

          <div>
            <label class="block text-sm font-medium mb-1">Type of Scam</label>
            <select name="type" required class="w-full border rounded-lg px-3 py-2">
              <option value="">Select a type</option>
              <option>Investment / Crypto</option>
              <option>Romance / Dating</option>
              <option>Tech Support</option>
              <option>Phishing Email / SMS</option>
              <option>Online Purchase / Marketplace</option>
              <option>Impersonation (bank, gov, etc.)</option>
              <option>Other</option>
            </select>
          </div>

          <div>
            <label class="block text-sm font-medium mb-1">Where did it happen?</label>
            <input type="text" name="channel" placeholder="WhatsApp, Instagram, Email, Website URL"
                   class="w-full border rounded-lg px-3 py-2">
          </div>

          <div>
            <label class="block text-sm font-medium mb-1">Approximate date</label>
            <input type="date" name="date" class="w-full border rounded-lg px-3 py-2">
          </div>

          <div>
            <label class="block text-sm font-medium mb-1">Amount involved (if any)</label>
            <input type="number" name="amount" min="0" step="0.01" placeholder="e.g., 2500"
                   class="w-full border rounded-lg px-3 py-2">
          </div>

          <div>
            <label class="block text-sm font-medium mb-1">Describe what happened</label>
            <textarea name="details" required rows="5"
                      class="w-full border rounded-lg px-3 py-2"
                      placeholder="Include how you were contacted, what they said, and any usernames or links."></textarea>
          </div>

          <button type="submit"
                  class="w-full bg-emerald-500 hover:bg-emerald-600 text-white py-3 rounded-lg font-semibold">
            Submit Report
          </button>

        </form>
      </div>
    </section>

    <!-- Help -->
    <section id="help" class="py-10 bg-slate-900 text-white">
      <div class="max-w-5xl mx-auto px-4">
        <h3 class="text-2xl font-bold mb-3">If you’ve lost money</h3>
        <ul class="list-disc list-inside text-sm text-slate-200 space-y-1">
          <li>Contact your bank immediately.</li>
          <li>Change passwords and enable 2FA.</li>
          <li>Report to your local cybercrime unit.</li>
          <li>Be cautious of “recovery services” asking for fees.</li>
        </ul>
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

  <!-- GSAP EFFECTS -->
  <script>
    document.getElementById('year').textContent = new Date().getFullYear();

    gsap.registerPlugin(ScrollTrigger);

    // Fade-in background
    gsap.from("#bgImage", {
      opacity: 0,
      duration: 1.5,
      ease: "power2.out"
    });

    // Parallax effect
    gsap.to("#bgImage", {
      y: 120,
      scale: 1.2,
      ease: "none",
      scrollTrigger: {
        trigger: "#about",
        start: "top top",
        end: "bottom top",
        scrub: true
      }
    });

    // Form submit → redirect
    document.getElementById('scamForm').addEventListener('submit', function (e) {
      e.preventDefault();
      const ref = "SR-" + Math.floor(100000 + Math.random() * 900000);
      sessionStorage.setItem("scamReportRef", ref);
      window.location.href = "thank-you.html";
    });
  </script>

</body>
</html>
