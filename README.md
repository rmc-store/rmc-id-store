import React, { useState } from 'react';

// Komponen untuk Halaman Utama
const HomePage = ({ services, onViewService }) => {
  return (
    <div className="container mx-auto p-4 md:p-8">
      <h1 className="text-4xl font-bold text-center mb-10 text-gray-800">Rico Maulana Creation</h1>
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        {services.map(service => (
          <div key={service.id} className="bg-white rounded-lg shadow-lg overflow-hidden transform transition-transform duration-300 hover:scale-105">
            <img
              src={service.imageUrl}
              alt={service.name}
              className="w-full h-48 object-cover"
              onError={(e) => { e.target.onerror = null; e.target.src = `https://placehold.co/400x300/F0F0F0/888888?text=Jasa+${service.id}`; }} // Placeholder jika gambar gagal dimuat
            />
            <div className="p-6">
              <h2 className="text-2xl font-semibold text-gray-900 mb-2">{service.name}</h2>
              <p className="text-gray-600 text-sm mb-4 line-clamp-3">{service.shortDescription}</p>
              <button
                onClick={() => onViewService(service)}
                className="w-full bg-blue-600 text-white py-2 px-4 rounded-md hover:bg-blue-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50"
              >
                Lihat Detail
              </button>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
};

// Komponen untuk Halaman Detail Jasa
const ServiceDetailPage = ({ service, onAddToCart, onBack }) => {
  const [currentImage, setCurrentImage] = useState(service.galleryImages[0] || service.imageUrl);

  return (
    <div className="container mx-auto p-4 md:p-8">
      <button
        onClick={onBack}
        className="mb-6 flex items-center text-blue-600 hover:text-blue-800 transition-colors duration-300"
      >
        <svg className="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
        Kembali ke Beranda
      </button>

      <div className="bg-white rounded-lg shadow-lg overflow-hidden p-6 md:p-10">
        <h1 className="text-3xl md:text-4xl font-bold text-gray-900 mb-6">{service.name}</h1>

        {/* Galeri Gambar */}
        <div className="mb-8">
          <img
            src={currentImage}
            alt={service.name}
            className="w-full h-80 md:h-96 object-contain rounded-lg mb-4 border border-gray-200"
            onError={(e) => { e.target.onerror = null; e.target.src = `https://placehold.co/800x600/F0F0F0/888888?text=Jasa+${service.id}+Detail`; }}
          />
          <div className="flex flex-wrap gap-3 justify-center">
            {service.galleryImages.map((img, index) => (
              <img
                key={index}
                src={img}
                alt={`${service.name} - Gambar ${index + 1}`}
                className={`w-20 h-20 object-cover rounded-md cursor-pointer border-2 ${currentImage === img ? 'border-blue-500' : 'border-transparent'} hover:border-blue-400 transition-all duration-200`}
                onClick={() => setCurrentImage(img)}
                onError={(e) => { e.target.onerror = null; e.target.src = `https://placehold.co/80x80/F0F0F0/888888?text=Img${index+1}`; }}
              />
            ))}
          </div>
        </div>

        {/* Deskripsi Jasa */}
        <div className="prose max-w-none text-gray-700 mb-8 leading-relaxed">
          <h3 className="text-2xl font-semibold text-gray-800 mb-3">Deskripsi Jasa</h3>
          <p>{service.longDescription}</p>
          <ul className="list-disc list-inside mt-4">
            <li>Waktu Pengerjaan: {service.duration}</li>
            <li>Harga Mulai: <span className="font-bold text-green-600">Rp {service.price.toLocaleString('id-ID')}</span></li>
            {/* Tambahkan poin-poin lain jika ada */}
          </ul>
        </div>

        <button
          onClick={() => onAddToCart(service)}
          className="w-full bg-green-600 text-white py-3 px-6 rounded-md text-lg font-semibold hover:bg-green-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-opacity-50"
        >
          Tambahkan ke Keranjang
        </button>
      </div>
    </div>
  );
};

// Komponen untuk Halaman Checkout
const CheckoutPage = ({ cartItems, onBackToHome, onClearCart }) => {
  const [formData, setFormData] = useState({ name: '', email: '', notes: '' });
  const [orderPlaced, setOrderPlaced] = useState(false);

  const totalPrice = cartItems.reduce((sum, item) => sum + item.price, 0);

  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    // Di sini Anda bisa mengintegrasikan dengan backend untuk memproses pesanan
    console.log('Pesanan Ditempatkan:', { items: cartItems, customerInfo: formData });
    setOrderPlaced(true);
    onClearCart(); // Kosongkan keranjang setelah pesanan ditempatkan
  };

  return (
    <div className="container mx-auto p-4 md:p-8">
      <h1 className="text-4xl font-bold text-center mb-10 text-gray-800">Checkout Pesanan</h1>

      {orderPlaced ? (
        <div className="bg-white rounded-lg shadow-lg p-8 text-center">
          <h2 className="text-3xl font-semibold text-green-600 mb-4">Pesanan Anda Berhasil Ditempatkan!</h2>
          <p className="text-gray-700 mb-6">Terima kasih telah berbelanja di toko kami. Kami akan segera menghubungi Anda.</p>
          <button
            onClick={onBackToHome}
            className="bg-blue-600 text-white py-2 px-6 rounded-md hover:bg-blue-700 transition-colors duration-300"
          >
            Kembali ke Beranda
          </button>
        </div>
      ) : (
        <div className="bg-white rounded-lg shadow-lg p-6 md:p-10">
          <h2 className="text-2xl font-semibold text-gray-900 mb-6">Ringkasan Keranjang</h2>
          {cartItems.length === 0 ? (
            <p className="text-gray-600 text-center mb-6">Keranjang Anda kosong. Silakan kembali ke beranda untuk memilih jasa.</p>
          ) : (
            <>
              <ul className="mb-6 border-b border-gray-200 pb-4">
                {cartItems.map(item => (
                  <li key={item.id} className="flex justify-between items-center py-2 border-b border-gray-100 last:border-b-0">
                    <span className="text-gray-700 font-medium">{item.name}</span>
                    <span className="text-gray-800">Rp {item.price.toLocaleString('id-ID')}</span>
                  </li>
                ))}
              </ul>
              <div className="flex justify-between items-center text-xl font-bold text-gray-900 mb-8">
                <span>Total:</span>
                <span>Rp {totalPrice.toLocaleString('id-ID')}</span>
              </div>

              <h2 className="text-2xl font-semibold text-gray-900 mb-6">Detail Pelanggan</h2>
              <form onSubmit={handleSubmit}>
                <div className="mb-4">
                  <label htmlFor="name" className="block text-gray-700 text-sm font-bold mb-2">Nama Lengkap:</label>
                  <input
                    type="text"
                    id="name"
                    name="name"
                    value={formData.name}
                    onChange={handleChange}
                    className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline focus:border-blue-500"
                    required
                  />
                </div>
                <div className="mb-4">
                  <label htmlFor="email" className="block text-gray-700 text-sm font-bold mb-2">Email:</label>
                  <input
                    type="email"
                    id="email"
                    name="email"
                    value={formData.email}
                    onChange={handleChange}
                    className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline focus:border-blue-500"
                    required
                  />
                </div>
                <div className="mb-6">
                  <label htmlFor="notes" className="block text-gray-700 text-sm font-bold mb-2">Catatan (Opsional):</label>
                  <textarea
                    id="notes"
                    name="notes"
                    value={formData.notes}
                    onChange={handleChange}
                    rows="4"
                    className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline focus:border-blue-500"
                  ></textarea>
                </div>
                <div className="flex justify-between items-center">
                  <button
                    type="button"
                    onClick={onBackToHome}
                    className="bg-gray-500 text-white py-2 px-4 rounded-md hover:bg-gray-600 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-gray-400 focus:ring-opacity-50"
                  >
                    Lanjutkan Belanja
                  </button>
                  <button
                    type="submit"
                    className="bg-blue-600 text-white py-2 px-6 rounded-md hover:bg-blue-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50"
                  >
                    Pesan Sekarang
                  </button>
                </div>
              </form>
            </>
          )}
        </div>
      )}
    </div>
  );
};

// Komponen Utama Aplikasi
const App = () => {
  const [view, setView] = useState('home'); // 'home', 'serviceDetail', 'checkout'
  const [selectedService, setSelectedService] = useState(null);
  const [cartItems, setCartItems] = useState([]);

  // Data contoh jasa online
  const services = [
    {
      id: 'web-design',
      name: 'Desain Website Kustom',
      shortDescription: 'Membangun website yang unik dan responsif sesuai kebutuhan bisnis Anda.',
      longDescription: 'Kami menawarkan jasa desain website kustom dari awal hingga akhir, memastikan setiap elemen dirancang untuk mencerminkan identitas merek Anda dan memberikan pengalaman pengguna yang optimal. Layanan ini mencakup konsultasi, wireframing, desain UI/UX, pengembangan front-end, dan optimasi SEO dasar. Cocok untuk startup, bisnis kecil, atau individu yang menginginkan kehadiran online yang profesional dan efektif.',
      price: 5000000,
      duration: '2-4 Minggu',
      imageUrl: 'https://placehold.co/600x400/E0E0E0/333333?text=Desain+Website',
      galleryImages: [
        'https://placehold.co/600x400/D0D0D0/444444?text=Website+Contoh+1',
        'https://placehold.co/600x400/C0C0C0/555555?text=Website+Contoh+2',
        'https://placehold.co/600x400/B0B0B0/666666?text=Website+Contoh+3',
      ],
    },
    {
      id: 'seo-optimization',
      name: 'Optimasi SEO & Peringkat',
      shortDescription: 'Meningkatkan visibilitas website Anda di mesin pencari untuk menjangkau lebih banyak pelanggan.',
      longDescription: 'Jasa optimasi SEO kami dirancang untuk meningkatkan peringkat website Anda di hasil pencarian Google dan mesin pencari lainnya. Kami melakukan analisis kata kunci mendalam, optimasi on-page (konten, meta deskripsi, struktur), optimasi teknis (kecepatan, mobile-friendliness), dan strategi backlink. Tujuannya adalah untuk menarik lalu lintas organik berkualitas tinggi ke website Anda dan meningkatkan konversi.',
      price: 2500000,
      duration: '1 Bulan (berkelanjutan)',
      imageUrl: 'https://placehold.co/600x400/D0D0D0/444444?text=Optimasi+SEO',
      galleryImages: [
        'https://placehold.co/600x400/C0C0C0/555555?text=SEO+Analisis',
        'https://placehold.co/600x400/B0B0B0/666666?text=SEO+Keyword',
      ],
    },
    {
      id: 'social-media-management',
      name: 'Manajemen Media Sosial',
      shortDescription: 'Mengelola akun media sosial Anda untuk membangun merek dan berinteraksi dengan audiens.',
      longDescription: 'Kami menyediakan jasa manajemen media sosial lengkap, mulai dari pembuatan strategi konten, penjadwalan posting, interaksi dengan audiens, hingga pelaporan performa. Kami membantu Anda memilih platform yang tepat (Instagram, Facebook, Twitter, LinkedIn), membuat konten yang menarik, dan membangun komunitas online yang loyal. Fokus kami adalah meningkatkan engagement, brand awareness, dan pada akhirnya, penjualan.',
      price: 3000000,
      duration: '1 Bulan (berkelanjutan)',
      imageUrl: 'https://i.ibb.co/Pvqr24n5/quality-restoration-20250701191557928.jpg', // URL foto diubah menjadi gambar utama di sini
      galleryImages: [
        'https://i.ibb.co/Pvqr24n5/quality-restoration-20250701191557928.jpg', // URL foto ditambahkan di sini
        'https://placehold.co/600x400/B0B0B0/666666?text=Sosmed+Konten',
        'https://placehold.co/600x400/A0A0A0/777777?text=Sosmed+Engagement',
      ],
    },
  ];

  const handleViewService = (service) => {
    setSelectedService(service);
    setView('serviceDetail');
  };

  const handleAddToCart = (service) => {
    setCartItems([...cartItems, service]);
    setView('checkout'); // Langsung ke halaman checkout setelah menambahkan ke keranjang
  };

  const handleBackToHome = () => {
    setView('home');
    setSelectedService(null);
  };

  const handleClearCart = () => {
    setCartItems([]);
  };

  return (
    <div className="min-h-screen bg-gray-100 font-sans text-gray-900">
      {/* Tailwind CSS CDN */}
      <script src="https://cdn.tailwindcss.com"></script>
      {/* Font Inter */}
      <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet" />
      <style>
        {`
          body {
            font-family: 'Inter', sans-serif;
          }
        `}
      </style>

      {/* Header */}
      <header className="bg-white shadow-md py-4">
        <div className="container mx-auto flex justify-between items-center px-4 md:px-8">
          <div className="text-2xl font-bold text-blue-600">rmc.id</div>
          <nav>
            <ul className="flex space-x-6">
              <li>
                <button onClick={handleBackToHome} className="text-gray-700 hover:text-blue-600 transition-colors duration-300">Beranda</button>
              </li>
              <li>
                <button onClick={() => setView('checkout')} className="text-gray-700 hover:text-blue-600 transition-colors duration-300 relative">
                  Keranjang ({cartItems.length})
                  {cartItems.length > 0 && (
                    <span className="absolute -top-2 -right-2 bg-red-500 text-white text-xs rounded-full h-5 w-5 flex items-center justify-center">
                      {cartItems.length}
                    </span>
                  )}
                </button>
              </li>
            </ul>
          </nav>
        </div>
      </header>

      {/* Konten Utama Berdasarkan Tampilan */}
      <main className="py-10">
        {view === 'home' && (
          <HomePage services={services} onViewService={handleViewService} />
        )}
        {view === 'serviceDetail' && selectedService && (
          <ServiceDetailPage
            service={selectedService}
            onAddToCart={handleAddToCart}
            onBack={handleBackToHome}
          />
        )}
        {view === 'checkout' && (
          <CheckoutPage
            cartItems={cartItems}
            onBackToHome={handleBackToHome}
            onClearCart={handleClearCart}
          />
        )}
      </main>

      {/* Footer */}
      <footer className="bg-gray-800 text-white py-6 mt-10">
        <div className="container mx-auto text-center text-gray-400">
          &copy; {new Date().getFullYear()} rmc.id. Hak Cipta Dilindungi.
        </div>
      </footer>
    </div>
  );
};

export default App;
