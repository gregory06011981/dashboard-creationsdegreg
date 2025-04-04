// 📁 dashboard-creationsdegreg - Initial project setup for Gregory

// ✅ Tech used: Next.js, React, Tailwind CSS, Airtable API

// 📌 Pages structure:
// - /dashboard (home page)
// - /commandes (view & manage orders from Airtable)
// - /taches (task management for yourself)
// - /clients (placeholder for client management)
// - /bijoux (placeholder for product management)

// === pages/index.tsx (redirect to dashboard) ===
import { useEffect } from 'react';
import { useRouter } from 'next/router';

export default function Home() {
  const router = useRouter();
  useEffect(() => {
    router.replace('/dashboard');
  }, [router]);
  return null;
}

// === pages/dashboard.tsx ===
export default function Dashboard() {
  return (
    <div className="p-6 text-center">
      <h1 className="text-3xl font-bold">Bienvenue Greg 👋</h1>
      <p className="mt-4 text-gray-600">Voici ton tableau de bord personnel.</p>
    </div>
  );
}

// === pages/commandes.tsx ===
import { useEffect, useState } from 'react';

export default function Commandes() {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('/api/commandes')
      .then(res => res.json())
      .then(setData)
      .finally(() => setLoading(false));
  }, []);

  return (
    <div className="p-6">
      <h1 className="text-2xl font-bold mb-4">Commandes</h1>
      {loading ? <p>Chargement...</p> : (
        <ul className="space-y-4">
          {data.map((cmd: any) => (
            <li key={cmd.id} className="border rounded-lg p-4">
              <p><strong>Client :</strong> {cmd.fields.Nom || '—'}</p>
              <p><strong>Produit :</strong> {cmd.fields.Produit || '—'}</p>
              <p><strong>Statut :</strong> {cmd.fields.Statut || '—'}</p>
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}

// === pages/api/commandes.ts ===
export default async function handler(req, res) {
  const AIRTABLE_API_KEY = process.env.AIRTABLE_API_KEY;
  const BASE_ID = 'appt7Ss9GbdPXTVjQ';
  const TABLE_NAME = 'Commandes';

  const response = await fetch(`https://api.airtable.com/v0/${BASE_ID}/${TABLE_NAME}`, {
    headers: {
      Authorization: `Bearer ${AIRTABLE_API_KEY}`,
    },
  });

  const data = await response.json();
  res.status(200).json(data.records);
}

// === tailwind.config.js ===
module.exports = {
  content: ['./pages/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
};
