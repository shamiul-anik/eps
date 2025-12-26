<script setup>
  
document.addEventListener('DOMContentLoaded', function () {
	// Theme Toggle Icons
	const themeToggleDarkIcon = document.getElementById('theme-toggle-dark-icon');
	const themeToggleLightIcon = document.getElementById('theme-toggle-light-icon');
	const themeToggleDarkIconMobile = document.getElementById('theme-toggle-dark-icon-mobile');
	const themeToggleLightIconMobile = document.getElementById('theme-toggle-light-icon-mobile');

	// Initial icon state
	if (document.documentElement.classList.contains('dark')) {
		themeToggleLightIcon && themeToggleLightIcon.classList.remove('hidden');
		themeToggleLightIconMobile && themeToggleLightIconMobile.classList.remove('hidden');
	} else {
		themeToggleDarkIcon && themeToggleDarkIcon.classList.remove('hidden');
		themeToggleDarkIconMobile && themeToggleDarkIconMobile.classList.remove('hidden');
	}

	function toggleTheme() {
		// Determine current state and flip it
		const isDark = document.documentElement.classList.contains('dark');
		if (isDark) {
			document.documentElement.classList.remove('dark');
			localStorage.setItem('color-theme', 'light');
		} else {
			document.documentElement.classList.add('dark');
			localStorage.setItem('color-theme', 'dark');
		}

		// Sync icons (defensive checks)
		if (themeToggleDarkIcon) themeToggleDarkIcon.classList.toggle('hidden');
		if (themeToggleLightIcon) themeToggleLightIcon.classList.toggle('hidden');
		if (themeToggleDarkIconMobile) themeToggleDarkIconMobile.classList.toggle('hidden');
		if (themeToggleLightIconMobile) themeToggleLightIconMobile.classList.toggle('hidden');
	}

	document.getElementById('theme-toggle')?.addEventListener('click', toggleTheme);
	document.getElementById('theme-toggle-mobile')?.addEventListener('click', toggleTheme);

	// Reveal Animations with stagger
	const observerOptions = { threshold: 0.12 };
	const revealObserver = new IntersectionObserver((entries, obs) => {
		entries.forEach(entry => {
			if (entry.isIntersecting) {
				entry.target.classList.add('active');
				obs.unobserve(entry.target);
			}
		});
	}, observerOptions);

	document.querySelectorAll('.reveal').forEach((el, i) => {
		// Staggered delay (modulo to avoid very long delays)
		const delay = (i % 10) * 90; // up to 900ms
		el.style.transitionDelay = `${delay}ms`;
		revealObserver.observe(el);
	});

	// Navbar Scroll Effect
	window.addEventListener('scroll', () => {
		const navbar = document.getElementById('navbar');
		if (!navbar) return;
		if (window.scrollY > 50) {
			navbar.classList.add('py-2', 'shadow-lg');
			navbar.classList.remove('py-4');
		} else {
			navbar.classList.add('py-4');
			navbar.classList.remove('py-2', 'shadow-lg');
		}
	});

	// Mobile Menu Logic
	const menuBtn = document.getElementById('menuBtn');
	const closeBtn = document.getElementById('closeBtn');
	const mobileMenu = document.getElementById('mobileMenu');
	const mobileLinks = document.querySelectorAll('.mobile-link');

	menuBtn?.addEventListener('click', () => {
		mobileMenu.classList.remove('hidden');
		document.body.style.overflow = 'hidden';
	});

	const closeMenu = () => {
		mobileMenu.classList.add('hidden');
		document.body.style.overflow = 'auto';
	};

	closeBtn?.addEventListener('click', closeMenu);
	mobileLinks.forEach(link => link.addEventListener('click', closeMenu));

	// Contact Form Handling
	document.getElementById('contactForm')?.addEventListener('submit', (e) => {
		e.preventDefault();
		const btn = e.target.querySelector('button');
		if (!btn) return;
		const originalText = btn.innerText;
		btn.innerText = 'Message Sent! ✅';
		btn.classList.replace('bg-blue-600', 'bg-green-600');
		e.target.reset();
		setTimeout(() => {
			btn.innerText = originalText;
			btn.classList.replace('bg-green-600', 'bg-blue-600');
		}, 3000);
	});
});

</script>

<template>
  <!-- Navigation -->
	<nav class="fixed w-full z-50 glass transition-all duration-300 py-4" id="navbar">
		<div class="container mx-auto px-6 flex justify-between items-center">
			<div class="flex items-center space-x-2 gap-1">
				<img src="/src/assets/logo.png" alt="Logo" class="w-12 h-12 bg-blue-600 rounded-full flex items-center justify-center text-white font-bold text-xl"></img>
				<div class="leading-tight">
					<h1 class="font-bold text-lg text-blue-900 dark:text-blue-400">EBAC Public School</h1>
					<p class="text-xs text-gray-500 dark:text-gray-400 font-medium tracking-widest uppercase">English Version</p>
				</div>
			</div>

			<div class="hidden md:flex space-x-8 items-center font-medium">
				<a href="#home" class="hover:text-blue-600 dark:hover:text-blue-400 transition">Home</a>
				<a href="#about" class="hover:text-blue-600 dark:hover:text-blue-400 transition">About</a>
				<a href="#academics" class="hover:text-blue-600 dark:hover:text-blue-400 transition">Academics</a>

				<!-- Dark Mode Toggle Desktop -->
				<button id="theme-toggle" type="button" class="text-gray-500 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700 focus:outline-none focus:ring-4 focus:ring-gray-200 dark:focus:ring-gray-700 rounded-lg text-sm p-2.5 transition">
					<i id="theme-toggle-dark-icon" class="fas fa-moon hidden"></i>
					<i id="theme-toggle-light-icon" class="fas fa-sun hidden"></i>
				</button>

				<a href="#contact" class="px-5 py-2 bg-blue-600 text-white rounded-full hover:bg-blue-700 transition">Admission</a>
			</div>

			<!-- Mobile Menu & Theme Button -->
			<div class="md:hidden flex items-center space-x-4">
				<button id="theme-toggle-mobile" type="button" class="text-gray-500 dark:text-gray-400 text-xl p-2">
					<i id="theme-toggle-dark-icon-mobile" class="fas fa-moon hidden"></i>
					<i id="theme-toggle-light-icon-mobile" class="fas fa-sun hidden"></i>
				</button>
				<button class="text-2xl" id="menuBtn">
					<i class="fas fa-bars"></i>
				</button>
			</div>
		</div>
	</nav>

	<!-- Mobile Menu -->
	<div id="mobileMenu" class="fixed inset-0 bg-white dark:bg-gray-900 z-[60] hidden flex flex-col items-center justify-center space-y-8 text-2xl font-bold transition-colors">
		<button class="absolute top-6 right-6 text-3xl dark:text-white" id="closeBtn">&times;</button>
		<a href="#home" class="mobile-link dark:text-white">Home</a>
		<a href="#about" class="mobile-link dark:text-white">About Us</a>
		<a href="#academics" class="mobile-link dark:text-white">Academics</a>
		<a href="#contact" class="mobile-link dark:text-white">Contact</a>
	</div>

	<!-- Hero Section -->
	<section id="home" class="relative min-h-screen flex items-center pt-20 overflow-hidden">
		<div class="absolute inset-0 z-0">
			<div class="absolute inset-0 bg-gradient-to-r from-blue-900/90 to-blue-800/40 dark:from-black/80 dark:to-blue-900/50"></div>
			<img src="https://images.unsplash.com/photo-1523050854058-8df90110c9f1?ixlib=rb-4.0.3&auto=format&fit=crop&w=1920&q=80" alt="School Campus" class="w-full h-full object-cover">
		</div>

		<div class="container mx-auto px-6 relative z-10">
			<div class="max-w-3xl text-white reveal">
				<span class="inline-block px-4 py-1 rounded-full bg-blue-500/20 border border-blue-400/30 text-blue-200 text-sm font-semibold mb-6">Empowering Future Leaders</span>
				<h1 class="text-5xl md:text-7xl font-bold mb-6 leading-tight">Nurturing Excellence in Education</h1>
				<p class="text-xl text-blue-100 mb-8 max-w-xl leading-relaxed">
					EBAC Public School (English Version) provides a global standard of education in Savar, Dhaka, fostering creativity, critical thinking, and character development.
				</p>
				<div class="flex flex-col sm:flex-row gap-4">
					<a href="#about" class="px-8 py-4 bg-blue-600 hover:bg-blue-700 text-white rounded-xl font-bold transition flex items-center justify-center">
						Explore Our School <i class="fas fa-arrow-right ml-2"></i>
					</a>
					<a href="#contact" class="px-8 py-4 bg-white/10 hover:bg-white/20 backdrop-blur-md border border-white/30 text-white rounded-xl font-bold transition flex items-center justify-center">
						Online Admission
					</a>
				</div>
			</div>
		</div>
	</section>

	<!-- Features -->
	<section class="py-24 bg-white dark:bg-gray-900 transition-colors">
		<div class="container mx-auto px-6">
			<div class="grid grid-cols-1 md:grid-cols-3 gap-12">
				<div class="reveal group">
					<div class="w-16 h-16 bg-blue-100 dark:bg-blue-900/30 rounded-2xl flex items-center justify-center text-blue-600 dark:text-blue-400 text-2xl mb-6 group-hover:scale-110 transition-transform">
						<i class="fas fa-chalkboard-teacher"></i>
					</div>
					<h3 class="text-2xl font-bold mb-4 dark:text-white">Expert Educators</h3>
					<p class="text-gray-600 dark:text-gray-400 leading-relaxed">Our faculty consists of highly qualified professionals dedicated to personalized student success and English language proficiency.</p>
				</div>
				<div class="reveal group" style="transition-delay: 100ms;">
					<div class="w-16 h-16 bg-green-100 dark:bg-green-900/30 rounded-2xl flex items-center justify-center text-green-600 dark:text-green-400 text-2xl mb-6 group-hover:scale-110 transition-transform">
						<i class="fas fa-laptop-code"></i>
					</div>
					<h3 class="text-2xl font-bold mb-4 dark:text-white">Modern Facilities</h3>
					<p class="text-gray-600 dark:text-gray-400 leading-relaxed">Equipped with digital classrooms, modern labs, and a resource-rich library to support 21st-century learning.</p>
				</div>
				<div class="reveal group" style="transition-delay: 200ms;">
					<div class="w-16 h-16 bg-purple-100 dark:bg-purple-900/30 rounded-2xl flex items-center justify-center text-purple-600 dark:text-purple-400 text-2xl mb-6 group-hover:scale-110 transition-transform">
						<i class="fas fa-football-ball"></i>
					</div>
					<h3 class="text-2xl font-bold mb-4 dark:text-white">Holistic Growth</h3>
					<p class="text-gray-600 dark:text-gray-400 leading-relaxed">We balance academics with vibrant extracurricular activities, sports, and cultural programs for all-round development.</p>
				</div>
			</div>
		</div>
	</section>

	<!-- About Section -->
	<section id="about" class="py-24 bg-gray-50 dark:bg-gray-800/50 overflow-hidden transition-colors">
		<div class="container mx-auto px-6">
			<div class="flex flex-col lg:flex-row items-center gap-16">
				<div class="lg:w-1/2 reveal">
					<div class="relative">
						<img src="https://images.unsplash.com/photo-1544717297-fa154da09f9d?ixlib=rb-4.0.3&auto=format&fit=crop&w=800&q=80" alt="Students" class="rounded-3xl shadow-2xl relative z-10">
						<div class="absolute -bottom-6 -right-6 w-64 h-64 bg-blue-600 rounded-3xl -z-0"></div>
						<div class="absolute top-1/2 -left-12 transform -translate-y-1/2 bg-white dark:bg-gray-800 p-6 rounded-2xl shadow-xl z-20 hidden md:block">
							<p class="text-blue-600 dark:text-blue-400 font-bold text-4xl">15+</p>
							<p class="text-gray-500 dark:text-gray-400 font-medium">Years of Excellence</p>
						</div>
					</div>
				</div>
				<div class="lg:w-1/2 reveal">
					<h2 class="text-4xl font-bold mb-6 text-blue-900 dark:text-blue-400 leading-tight">About EBAC Public School</h2>
					<p class="text-gray-600 dark:text-gray-400 text-lg mb-6 leading-relaxed">
						EBAC Public School (English Version) is a premier educational institution located in Savar, Dhaka. We are committed to providing an environment where students can flourish intellectually and socially.
					</p>
					<ul class="space-y-4 mb-8">
						<li class="flex items-center space-x-3">
							<i class="fas fa-check-circle text-blue-500"></i>
							<span class="font-medium text-gray-700 dark:text-gray-300">British Curriculum influenced English Version</span>
						</li>
						<li class="flex items-center space-x-3">
							<i class="fas fa-check-circle text-blue-500"></i>
							<span class="font-medium text-gray-700 dark:text-gray-300">Safe and Secure Campus with CCTV monitoring</span>
						</li>
						<li class="flex items-center space-x-3">
							<i class="fas fa-check-circle text-blue-500"></i>
							<span class="font-medium text-gray-700 dark:text-gray-300">Focus on Moral and Ethical Values</span>
						</li>
					</ul>
					<button class="px-8 py-4 border-2 border-blue-600 text-blue-600 dark:text-blue-400 dark:border-blue-400 font-bold rounded-xl hover:bg-blue-600 hover:text-white dark:hover:bg-blue-400 dark:hover:text-gray-900 transition">Read Our Story</button>
				</div>
			</div>
		</div>
	</section>

	<!-- Academics Section -->
	<section id="academics" class="py-24 bg-white dark:bg-gray-900 transition-colors">
		<div class="container mx-auto px-6 text-center">
			<h2 class="text-4xl font-bold mb-4 reveal dark:text-white">Academic Programs</h2>
			<p class="text-gray-500 dark:text-gray-400 max-w-2xl mx-auto mb-16 reveal">Tailored educational paths designed to challenge and inspire students at every level.</p>

			<div class="grid grid-cols-1 md:grid-cols-3 gap-8">
				<!-- Program 1 -->
				<div class="bg-blue-50 dark:bg-gray-800 p-8 rounded-3xl reveal border border-blue-100 dark:border-gray-700 hover:shadow-xl transition duration-300">
					<h3 class="text-2xl font-bold mb-4 text-blue-900 dark:text-blue-400">Pre-Primary</h3>
					<p class="text-gray-600 dark:text-gray-400 mb-6">Play-based learning for our youngest students to build foundation and curiosity.</p>
					<div class="text-sm font-semibold text-blue-600 dark:text-blue-500 uppercase tracking-widest">Ages 3 - 5</div>
				</div>
				<!-- Program 2 -->
				<div class="bg-blue-600 p-8 rounded-3xl reveal text-white shadow-lg scale-105">
					<h3 class="text-2xl font-bold mb-4">Primary Section</h3>
					<p class="text-blue-100 mb-6">Developing core skills in English, Math, and Science with a global perspective.</p>
					<div class="text-sm font-semibold text-white/80 uppercase tracking-widest">Class 1 - 5</div>
				</div>
				<!-- Program 3 -->
				<div class="bg-blue-50 dark:bg-gray-800 p-8 rounded-3xl reveal border border-blue-100 dark:border-gray-700 hover:shadow-xl transition duration-300">
					<h3 class="text-2xl font-bold mb-4 text-blue-900 dark:text-blue-400">Secondary Section</h3>
					<p class="text-gray-600 dark:text-gray-400 mb-6">Advanced academic rigors preparing students for board exams and beyond.</p>
					<div class="text-sm font-semibold text-blue-600 dark:text-blue-500 uppercase tracking-widest">Class 6 - 10</div>
				</div>
			</div>
		</div>
	</section>

	<!-- Contact Section -->
	<section id="contact" class="py-24 bg-blue-900 dark:bg-gray-950 text-white transition-colors">
		<div class="container mx-auto px-6">
			<div class="grid grid-cols-1 lg:grid-cols-2 gap-16">
				<div class="reveal">
					<h2 class="text-4xl font-bold mb-8">Get In Touch</h2>
					<p class="text-blue-100 dark:text-gray-400 mb-12 text-lg">Have questions about admissions or our curriculum? Our team is here to help you.</p>

					<div class="space-y-6">
						<div class="flex items-center space-x-4">
							<div class="w-12 h-12 bg-white/10 rounded-full flex items-center justify-center">
								<i class="fas fa-map-marker-alt"></i>
							</div>
							<div>
								<p class="font-bold">Address</p>
								<p class="text-blue-200 dark:text-gray-400">Savar, Dhaka, Bangladesh</p>
							</div>
						</div>
						<div class="flex items-center space-x-4">
							<div class="w-12 h-12 bg-white/10 rounded-full flex items-center justify-center">
								<i class="fas fa-phone"></i>
							</div>
							<div>
								<p class="font-bold">Phone</p>
								<p class="text-blue-200 dark:text-gray-400">+880 1234 567890</p>
							</div>
						</div>
						<div class="flex items-center space-x-4">
							<div class="w-12 h-12 bg-white/10 rounded-full flex items-center justify-center">
								<i class="fas fa-envelope"></i>
							</div>
							<div>
								<p class="font-bold">Email</p>
								<p class="text-blue-200 dark:text-gray-400">info@ebacschool.edu.bd</p>
							</div>
						</div>
					</div>
				</div>

				<div class="bg-white dark:bg-gray-800 rounded-3xl p-8 text-gray-900 dark:text-white reveal">
					<form id="contactForm" class="space-y-4">
						<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
							<div>
								<label class="block text-sm font-bold mb-2">Parent Name</label>
								<input type="text" class="w-full px-4 py-3 bg-gray-100 dark:bg-gray-700 dark:text-white rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-600 dark:focus:ring-blue-400 transition" placeholder="John Doe">
							</div>
							<div>
								<label class="block text-sm font-bold mb-2">Phone Number</label>
								<input type="tel" class="w-full px-4 py-3 bg-gray-100 dark:bg-gray-700 dark:text-white rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-600 dark:focus:ring-blue-400 transition" placeholder="01712-XXXXXX">
							</div>
						</div>
						<div>
							<label class="block text-sm font-bold mb-2">Student's Current Class</label>
							<select class="w-full px-4 py-3 bg-gray-100 dark:bg-gray-700 dark:text-white rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-600 dark:focus:ring-blue-400 transition">
								<option>Select Class</option>
								<option>Play/Nursery</option>
								<option>Class 1-5</option>
								<option>Class 6-10</option>
							</select>
						</div>
						<div>
							<label class="block text-sm font-bold mb-2">Message</label>
							<textarea rows="4" class="w-full px-4 py-3 bg-gray-100 dark:bg-gray-700 dark:text-white rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-600 dark:focus:ring-blue-400 transition" placeholder="How can we help?"></textarea>
						</div>
						<button type="submit" class="w-full py-4 bg-blue-600 text-white font-bold rounded-xl hover:bg-blue-700 transition transform hover:-translate-y-1">Send Message</button>
					</form>
				</div>
			</div>
		</div>
	</section>

	<!-- Footer -->
	<footer class="bg-gray-950 text-gray-400 py-12 border-t border-gray-900">
		<div class="container mx-auto px-6">
			<div class="flex flex-col md:flex-row justify-between items-center mb-12">
				<div class="flex items-center space-x-2 mb-8 md:mb-0">
					<div class="w-8 h-8 bg-blue-600 rounded flex items-center justify-center text-white font-bold">E</div>
					<span class="text-white font-bold text-xl">EBAC Public School</span>
				</div>
				<div class="flex space-x-6 text-xl">
					<a href="#" class="hover:text-blue-500 transition"><i class="fab fa-facebook"></i></a>
					<a href="#" class="hover:text-blue-500 transition"><i class="fab fa-twitter"></i></a>
					<a href="#" class="hover:text-blue-500 transition"><i class="fab fa-youtube"></i></a>
					<a href="#" class="hover:text-blue-500 transition"><i class="fab fa-linkedin"></i></a>
				</div>
			</div>
			<div class="text-center text-sm">
				<p>&copy; 2024 EBAC Public School (English Version). All rights reserved.</p>
			</div>
		</div>
	</footer>
</template>

<style scoped>

</style>
