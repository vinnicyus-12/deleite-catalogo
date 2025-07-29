// 📁 pages/index.js
import Head from 'next/head';
import Catalog from '@/components/Catalog';

export default function Home() {
  return (
    <>
      <Head>
        <title>Deleite | Catálogo de Joias em Prata</title>
      </Head>
      <main className="min-h-screen bg-blue-50 bg-[url('/textures/pastel-texture.png')] p-4">
        <Catalog />
      </main>
    </>
  );
}

// 📁 pages/admin.js
import Head from 'next/head';
import AdminPanel from '@/components/AdminPanel';

export default function Admin() {
  return (
    <>
      <Head>
        <title>Área Administrativa | Deleite</title>
      </Head>
      <main className="min-h-screen bg-blue-50 bg-[url('/textures/pastel-texture.png')] p-4">
        <AdminPanel />
      </main>
    </>
  );
}

// 📁 pages/produto/[code].js
import { useRouter } from 'next/router';
import Head from 'next/head';
import products from '@/data/products';

export default function ProductPage() {
  const router = useRouter();
  const { code } = router.query;
  const product = products.find(p => p.code === code);

  if (!product) return <p className="p-4">Produto não encontrado.</p>;

  return (
    <>
      <Head>
        <title>{product.title} | Deleite</title>
      </Head>
      <main className="min-h-screen bg-blue-50 bg-[url('/textures/pastel-texture.png')] p-4">
        <div className="max-w-4xl mx-auto bg-white shadow rounded-xl p-6 grid md:grid-cols-2 gap-6">
          <img src={product.image} alt={product.title} className="w-full h-auto rounded-md" />
          <div>
            <h1 className="text-2xl font-bold text-gray-800 mb-2">{product.title}</h1>
            <p className="text-gray-600 mb-1">Ref: {product.code}</p>
            <p className="text-lg font-semibold text-blue-600 mb-4">{product.price}</p>
            <p className="text-gray-700 mb-4">{product.description}</p>
            <button className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
              Adicionar ao carrinho
            </button>
          </div>
        </div>
      </main>
    </>
  );
}

// 📁 components/Catalog.js
import ProductCard from './ProductCard';
import products from '@/data/products';

export default function Catalog() {
  return (
    <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
      {products.map(product => (
        <ProductCard key={product.code} {...product} />
      ))}
    </div>
  );
}

// 📁 components/ProductCard.js
import { useRouter } from 'next/router';

export default function ProductCard({ title, price, code, image }) {
  const router = useRouter();
  return (
    <div
      onClick={() => router.push(`/produto/${code}`)}
      className="cursor-pointer rounded-xl shadow-md bg-white p-4 hover:shadow-lg transition"
    >
      <img src={image} alt={title} className="w-full h-48 object-cover rounded-md" />
      <h3 className="font-semibold mt-2 text-gray-800">{title}</h3>
      <p className="text-gray-600">{price}</p>
      <p className="text-xs text-gray-400">Ref: {code}</p>
    </div>
  );
}

// 📁 components/AdminPanel.js
import products from '@/data/products';

export default function AdminPanel() {
  return (
    <div className="bg-white shadow p-6 rounded-xl">
      <h1 className="text-2xl font-bold mb-4">Painel Administrativo</h1>
      <p className="mb-4">Gerencie seus produtos aqui (em breve: editar, excluir e adicionar).</p>
      <ul className="list-disc pl-5">
        {products.map(p => (
          <li key={p.code} className="mb-1">{p.title} - {p.price} (Ref: {p.code})</li>
        ))}
      </ul>
    </div>
  );
}

// 📁 data/products.js
const products = [
  {
    title: 'Anel Prata Minimalista',
    price: 'R$ 180,00',
    code: 'DEL001',
    image: '/img/anel1.png',
    description: 'Anel elegante de prata com design minimalista, ideal para uso diário.'
  },
  {
    title: 'Colar Estelar',
    price: 'R$ 220,00',
    code: 'DEL002',
    image: '/img/colar1.png',
    description: 'Colar delicado em prata com pingente de estrela, perfeito para ocasiões especiais.'
  }
];

export default products;

// 📁 tailwind.config.js
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx}',
    './components/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}

// 📁 styles/globals.css
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  @apply bg-blue-50 bg-[url('/textures/pastel-texture.png')];
  font-family: 'Inter', sans-serif;
}
