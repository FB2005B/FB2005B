import React, { useState, useEffect } from 'react';
import { 
  Search, Home, Wrench, Settings, Users, Star, 
  ArrowRight, MessageCircle, Calendar, Clock,
  CreditCard, MapPin, Phone, Mail, Bell, Menu,
  X, ChevronLeft, ChevronRight, User, LogOut,
  Heart, Clock8, DollarSign, AlertTriangle,
  CheckCircle, Filter, PlusCircle
} from 'lucide-react';

// Composant Notification Toast
const Toast = ({ message, type, onClose }) => {
  useEffect(() => {
    const timer = setTimeout(onClose, 3000);
    return () => clearTimeout(timer);
  }, [onClose]);

  const bgColor = {
    success: 'bg-green-500',
    error: 'bg-red-500',
    info: 'bg-blue-500'
  }[type];

  return (
    <div className={`fixed bottom-4 right-4 ${bgColor} text-white px-6 py-3 rounded-lg 
      shadow-lg transform transition-all duration-500 hover:scale-105 cursor-pointer
      animate-slide-in-right`}
      onClick={onClose}>
      {message}
    </div>
  );
};

// Composant Dashboard
const Dashboard = ({ isOpen, onClose }) => {
  const [activeTab, setActiveTab] = useState('reservations');
  
  const reservations = [
    { id: 1, service: 'Plomberie', date: '2024-05-01', status: 'En attente' },
    { id: 2, service: 'Ménage', date: '2024-05-03', status: 'Confirmé' }
  ];

  const favoris = [
    { id: 1, name: 'Jean P.', service: 'Plomberie', rating: 4.8 },
    { id: 2, name: 'Marie L.', service: 'Ménage', rating: 4.9 }
  ];

  if (!isOpen) return null;

  return (
    <div className="fixed inset-0 bg-black bg-opacity-50 z-50 flex justify-end transition-all duration-500">
      <div className="w-full max-w-md bg-white h-full p-6 transform transition-transform duration-500 animate-slide-in-right">
        <div className="flex justify-between items-center mb-8">
          <h2 className="text-2xl font-bold">Tableau de bord</h2>
          <button onClick={onClose} className="p-2 hover:bg-gray-100 rounded-full transition-colors">
            <X size={24} />
          </button>
        </div>

        {/* Stats rapides */}
        <div className="grid grid-cols-2 gap-4 mb-8">
          <div className="bg-blue-50 p-4 rounded-lg hover:shadow-lg transition-all duration-300 hover:scale-105">
            <div className="flex items-center gap-2 mb-2">
              <Calendar size={20} className="text-blue-600" />
              <span className="font-semibold">Réservations</span>
            </div>
            <span className="text-2xl font-bold">5</span>
          </div>
          <div className="bg-green-50 p-4 rounded-lg hover:shadow-lg transition-all duration-300 hover:scale-105">
            <div className="flex items-center gap-2 mb-2">
              <CheckCircle size={20} className="text-green-600" />
              <span className="font-semibold">Complétés</span>
            </div>
            <span className="text-2xl font-bold">12</span>
          </div>
        </div>

        {/* Tabs */}
        <div className="flex gap-4 mb-6 border-b">
          {['reservations', 'favoris', 'parametres'].map((tab) => (
            <button
              key={tab}
              onClick={() => setActiveTab(tab)}
              
              className={`pb-2 px-2 transition-colors ${
                activeTab === tab 
                ? 'border-b-2 border-blue-600 text-blue-600' 
                : 'text-gray-600 hover:text-blue-600'
              }`}
            >
              {tab.charAt(0).toUpperCase() + tab.slice(1)}
            </button>
          ))}
        </div>

        {/* Contenu des tabs */}
        <div className="overflow-y-auto h-[calc(100vh-300px)]">
          {activeTab === 'reservations' && (
            <div className="space-y-4">
              {reservations.map((res) => (
                <div key={res.id} 
                  className="bg-white border rounded-lg p-4 hover:shadow-md transition-all duration-300 
                    hover:scale-102 cursor-pointer">
                  <div className="flex justify-between items-center mb-2">
                    <span className="font-semibold">{res.service}</span>
                    <span className={`px-2 py-1 rounded-full text-sm ${
                      res.status === 'Confirmé' ? 'bg-green-100 text-green-800' : 'bg-yellow-100 text-yellow-800'
                    }`}>
                      {res.status}
                    </span>
                  </div>
                  <div className="text-sm text-gray-600">
                    <div className="flex items-center gap-2">
                      <Calendar size={16} />
                      <span>{res.date}</span>
                    </div>
                  </div>
                </div>
              ))}
            </div>
          )}

          {activeTab === 'favoris' && (
            <div className="space-y-4">
              {favoris.map((fav) => (
                <div key={fav.id} 
                  className="bg-white border rounded-lg p-4 hover:shadow-md transition-all duration-300 
                    hover:scale-102 group">
                  <div className="flex justify-between items-center">
                    <div>
                      <span className="font-semibold">{fav.name}</span>
                      <p className="text-sm text-gray-600">{fav.service}</p>
                    </div>
                    <div className="flex items-center gap-1">
                      <Star size={16} className="text-yellow-400 fill-current" />
                      <span>{fav.rating}</span>
                    </div>
                  </div>
                </div>
              ))}
            </div>
          )}

          {activeTab === 'parametres' && (
            <div className="space-y-4">
              <div className="border rounded-lg p-4 hover:bg-gray-50 transition-colors cursor-pointer">
                <div className="flex items-center gap-3">
                  <User size={20} />
                  <span>Modifier le profil</span>
                </div>
              </div>
              <div className="border rounded-lg p-4 hover:bg-gray-50 transition-colors cursor-pointer">
                <div className="flex items-center gap-3">
                  <Bell size={20} />
                  <span>Notifications</span>
                </div>
              </div>
              <div className="border rounded-lg p-4 hover:bg-gray-50 transition-colors cursor-pointer">
                <div className="flex items-center gap-3">
                  <CreditCard size={20} />
                  <span>Paiements</span>
                </div>
              </div>
            </div>
          )}
        </div>
      </div>
    </div>
  );
};

// Composant Filtre Services
const ServiceFilter = ({ onFilter }) => {
  const [isOpen, setIsOpen] = useState(false);
  const categories = ['Tous', 'Plomberie', 'Mécanique', 'Ménage'];
  const [selected, setSelected] = useState('Tous');

  return (
    <div className="relative">
      <button
        onClick={() => setIsOpen(!isOpen)}
        className="flex items-center gap-2 px-4 py-2 bg-white rounded-lg shadow hover:shadow-md 
          transition-all duration-300"
      >
        <Filter size={20} />
        <span>Filtrer</span>
      </button>

      {isOpen && (
        <div className="absolute top-full mt-2 w-48 bg-white rounded-lg shadow-lg p-2 
          transform transition-all duration-300 animate-fade-in">
          {categories.map((cat) => (
            <button
              key={cat}
              onClick={() => {
                setSelected(cat);
                onFilter(cat);
                setIsOpen(false);
              }}
              className={`w-full text-left px-4 py-2 rounded-md transition-colors ${
                selected === cat ? 'bg-blue-50 text-blue-600' : 'hover:bg-gray-50'
              }`}
            >
              {cat}
            </button>
          ))}
        </div>
      )}
    </div>
  );
};

// Style global pour les animations
const style = document.createElement('style');
style.textContent = `
  @keyframes slide-in-right {
    from { transform: translateX(100%); }
    to { transform: translateX(0); }
  }
  @keyframes fade-in {
    from { opacity: 0; }
    to { opacity: 1; }
  }
  @keyframes bounce-in {
    0% { transform: scale(0.3); opacity: 0; }
    50% { transform: scale(1.05); opacity: 0.8; }
    70% { transform: scale(0.9); opacity: 0.9; }
    100% { transform: scale(1); opacity: 1; }
  }
  .animate-slide-in-right {
    animation: slide-in-right 0.5s ease-out;
  }
  .animate-fade-in {
    animation: fade-in 0.3s ease-out;
  }
  .animate-bounce-in {
    animation: bounce-in 0.8s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  }
`;
document.head.appendChild(style);

// Composant Principal
const HomePage = () => {
  const [searchFocus, setSearchFocus] = useState(false);
  const [isDashboardOpen, setIsDashboardOpen] = useState(false);
  const [toast, setToast] = useState(null);
  const [selectedCategory, setSelectedCategory] = useState('Tous');

  const showToast = (message, type = 'info') => {
    setToast({ message, type });
  };

  const handleFilter = (category) => {
    setSelectedCategory(category);
    showToast(`Filtré par : ${category}`, 'info');
  };

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Header avec navigation avancée */}
      <header className="bg-white shadow-sm fixed w-full top-0 z-40">
        <nav className="container mx-auto px-4 py-4">
          <div className="flex justify-between items-center">
            <div className="text-xl font-bold text-blue-600 hover:scale-105 transition-transform cursor-pointer">
              ServicesHome
            </div>
            
            <div className="flex items-center gap-6">
              <ServiceFilter onFilter={handleFilter} />
              
              <button 
                onClick={() => {
                  setIsDashboardOpen(true);
                  showToast('Bienvenue dans votre tableau de bord', 'success');
                }}
                className="flex items-center gap-2 hover:text-blue-600 transition-colors"
              >
                <User size={20} />
                <span className="hidden md:inline">Mon Compte</span>
              </button>
              
              <button className="relative hover:scale-110 transition-transform">
                <Bell size={20} />
                <span className="absolute -top-1 -right-1 bg-red-500 text-white text-xs rounded-full w-4 h-4 flex items-center justify-center animate-bounce">
                  3
                </span>
              </button>
            </div>
          </div>
        </nav>
      </header>

      {/* Contenu principal */}
      <main className="pt-16">
        {/* Reste du contenu... */}
      </main>

      {/* Dashboard */}
      <Dashboard 
        isOpen={isDashboardOpen} 
        onClose={() => setIsDashboardOpen(false)} 
      />

      {/* Toast Notifications */}
      {toast && (
        <Toast 
          message={toast.message} 
          type={toast.type} 
          onClose={() => setToast(null)} 
        />
      )}
    </div>
  );
};

export default HomePage;




<!---
FB2005B/FB2005B is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
