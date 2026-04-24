import { useState, useEffect } from 'react';
import Head from 'next/head';

interface Resource {
  id: string;
  titre: string;
  url: string;
  type: string;
  motsCles: string;
  dateAjout: string;
  partenaire: string;
  notes: string;
  categorie: string;
}

export default function Home() {
  const [resources, setResources] = useState<Resource[]>([]);
  const [searchQuery, setSearchQuery] = useState('');
  const [selectedCategory, setSelectedCategory] = useState('');
  const [selectedType, setSelectedType] = useState('');
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function loadData() {
      try {
        const res = await fetch('/data.json');
        const data = await res.json();
        setResources(data);
      } catch (error) {
        console.error('Erreur chargement données:', error);
      } finally {
        setLoading(false);
      }
    }
    loadData();
  }, []);

  const filteredResources = resources.filter(res => {
    const matchesSearch = searchQuery === '' || 
      res.titre.toLowerCase().includes(searchQuery.toLowerCase()) ||
      res.motsCles.toLowerCase().includes(searchQuery.toLowerCase()) ||
      res.notes.toLowerCase().includes(searchQuery.toLowerCase());
    
    const matchesCategory = selectedCategory === '' || res.categorie === selectedCategory;
    const matchesType = selectedType === '' || res.type === selectedType;
    
    return matchesSearch && matchesCategory && matchesType;
  });

  const categories = Array.from(new Set(resources.map(r => r.categorie))).sort();
  const types = Array.from(new Set(resources.map(r => r.type))).sort();

  const getTypeIcon = (type: string) => {
    const icons: { [key: string]: string } = {
      'PDF': '📄',
      'Document': '📝',
      'Excel': '📊',
      'Image/Schéma': '🖼️',
      'Vidéo': '🎥',
      'Lien': '🔗',
      'Note': '📌',
      'Outil': '🛠️',
      'Autre': '📁'
    };
    return icons[type] || '📄';
  };

  const copyToClipboard = (text: string) => {
    navigator.clipboard.writeText(text);
    alert('Chemin copié !');
  };

  if (loading) {
    return (
      <div className="container">
        <h1>Chargement...</h1>
      </div>
    );
  }

  return (
    <>
      <Head>
        <title>PRIMFI Knowledge Hub</title>
        <meta name="description" content="Ressources et veille stratégique PRIMFI" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
      </Head>

      <div className="container">
        <header className="header">
          <h1>📚 PRIMFI Knowledge Hub</h1>
          <p>Ressources et veille stratégique</p>
        </header>

        <div className="search-section">
          <input
            type="text"
            placeholder="🔍 Rechercher par titre, mots-clés, notes..."
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
            className="search-input"
          />
        </div>

        <div className="filters">
          <div className="filter-group">
            <label>📂 Catégorie :</label>
            <select value={selectedCategory} onChange={(e) => setSelectedCategory(e.target.value)}>
              <option value="">Toutes</option>
              {categories.map(cat => (
                <option key={cat} value={cat}>{cat}</option>
              ))}
            </select>
            {selectedCategory && (
              <button onClick={() => setSelectedCategory('')} className="clear-btn">✕</button>
            )}
          </div>

          <div className="filter-group">
            <label>📋 Type :</label>
            <select value={selectedType} onChange={(e) => setSelectedType(e.target.value)}>
              <option value="">Tous</option>
              {types.map(type => (
                <option key={type} value={type}>{type}</option>
              ))}
            </select>
            {selectedType && (
              <button onClick={() => setSelectedType('')} className="clear-btn">✕</button>
            )}
          </div>

          {(selectedCategory || selectedType || searchQuery) && (
            <button 
              onClick={() => {
                setSearchQuery('');
                setSelectedCategory('');
                setSelectedType('');
              }}
              className="reset-btn"
            >
              Réinitialiser les filtres
            </button>
          )}
        </div>

        <div className="results-info">
          <strong>{filteredResources.length}</strong> / {resources.length} ressources
        </div>

        <div className="results">
          {filteredResources.length === 0 ? (
            <p className="no-results">Aucune ressource ne correspond à votre recherche.</p>
          ) : (
            filteredResources.map(res => (
              <div key={res.id} className="resource-card">
                <div className="resource-header">
                  <span className="type-icon">{getTypeIcon(res.type)}</span>
                  <h3>{res.titre}</h3>
                </div>
                <p className="resource-meta">
                  <strong>Type :</strong> {res.type} | <strong>Catégorie :</strong> {res.categorie}
                </p>
                {res.motsCles && (
                  <p className="resource-keywords">
                    <strong>Mots-clés :</strong> {res.motsCles}
                  </p>
                )}
                {res.partenaire && (
                  <p className="resource-partner">
                    <strong>Partenaire :</strong> {res.partenaire}
                  </p>
                )}
                {res.notes && (
                  <p className="resource-notes">{res.notes}</p>
                )}
                <div className="resource-footer">
                  <span className="date">{res.dateAjout}</span>
                  <button 
                    onClick={() => copyToClipboard(res.url)}
                    className="copy-btn"
                    title="Copier le chemin"
                  >
                    📋 Copier chemin
                  </button>
                </div>
              </div>
            ))
          )}
        </div>
      </div>
    </>
  );
}
