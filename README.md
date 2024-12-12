# -onelifegame-website.
import Image from 'next/image';
import Link from 'next/link';
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Accordion, AccordionContent, AccordionItem, AccordionTrigger } from "@/components/ui/accordion";
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import { useState, useEffect } from 'react';
import { motion } from 'framer-motion';
import { Sword, Shield, Wind, Braces, Users, Clock, ChevronRight } from 'lucide-react';
import { FactionCard } from './components/FactionCard';
import Head from 'next/head';
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: 'your-sentry-dsn',
  tracesSampleRate: 1.0,
});

export default function LandingPage() {
  const [hoveredFaction, setHoveredFaction] = useState(null);
  const [email, setEmail] = useState('');
  const [subscribed, setSubscribed] = useState(false);
  const [factions, setFactions] = useState([]);

  useEffect(() => {
    const fetchFactions = async () => {
      const res = await fetch('/api/factions');
      const data = await res.json();
      setFactions(data);
    };
    fetchFactions();
  }, []);

  const handleSubmit = async (e) => {
    e.preventDefault();
    const res = await fetch('/api/subscribe', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ email }),
    });
    if (res.ok) {
      setSubscribed(true);
    }
  };

  return (
    <>
      <Head>
        <title>OneLife Game</title>
        <meta name="description" content="OneLife - Tactical PvP game in a medieval fantasy world." />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <link rel="icon" href="/favicon.ico" />
      </Head>
      <div className="min-h-screen bg-[url('/background.jpg')] bg-cover bg-center bg-fixed text-white">
        <div className="bg-black bg-opacity-70 min-h-screen">
          <header className="container mx-auto px-4 py-8">
            <nav className="flex justify-between items-center">
              <Image src="/logo.svg" alt="OneLife Logo" width={150} height={50} />
              <div className="space-x-4">
                <Link href="#about" className="hover:text-red-500 transition-colors">О игре</Link>
                <Link href="#factions" className="hover:text-red-500 transition-colors">Фракции</Link>
                <Link href="#features" className="hover:text-red-500 transition-colors">Особенности</Link>
                <Link href="#mechanics" className="hover:text-red-500 transition-colors">Механики</Link>
                <Link href="#components" className="hover:text-red-500 transition-colors">Компоненты</Link>
                <Link href="#buy" className="hover:text-red-500 transition-colors">Купить</Link>
                <Button variant="outline" className="bg-red-600 text-white border-red-600 hover:bg-red-700 transition-colors">Купить сейчас</Button>
              </div>
            </nav>
          </header>

          <main className="container mx-auto px-4">
            <section className="text-center py-20">
              <motion.h1
                className="text-6xl font-bold mb-6 text-red-500"
                initial={{ opacity: 0, y: -50 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.5 }}
              >
                Добро пожаловать в OneLife
              </motion.h1>
              <motion.p
                className="text-2xl mb-8"
                initial={{ opacity: 0, y: -30 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.5, delay: 0.2 }}
              >
                Тактическая PvP игра в мире средневекового фэнтези
              </motion.p>
              <motion.div
                className="flex justify-center space-x-8 mb-8"
                initial={{ opacity: 0, y: -20 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.5, delay: 0.4 }}
              >
                <div className="flex items-center">
                  <Users className="w-6 h-6 mr-2" />
                  <span>2-4 игрока</span>
                </div>
                <div className="flex items-center">
                  <Clock className="w-6 h-6 mr-2" />
                  <span>60-120 минут</span>
                </div>
                <div className="flex items-center">
                  <span>Возраст: 12+</span>
                </div>
              </motion.div>
              <motion.div
                initial={{ opacity: 0, y: -20 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.5, delay: 0.6 }}
              >
                <Button size="lg" className="bg-red-600 hover:bg-red-700 text-xl px-8 py-4 transition-colors">Начать игру</Button>
              </motion.div>
            </section>

            <section id="about" className="py-20">
              <h2 className="text-4xl font-bold mb-6 text-center text-red-500">О игре</h2>
              <div className="flex flex-col md:flex-row items-center gap-8">
                <div className="md:w-1/2">
                  <p className="text-lg mb-4">
                    OneLife - это захватывающая тактическая PvP игра для 2-4 игроков,
                    сочетающая в себе элементы карточных и настольных игр в уникальном
                    сеттинге средневекового фэнтези.
                  </p>
                  <p className="text-lg mb-4">
                    Ваша цель - набрать 8 победных очков (16 в командной игре), уничтожая фигуры противников,
                    удерживая ключевые точки и захватывая флаги врага.
                  </p>
                  <p className="text-lg">
                    Управляйте отрядом из 6 фигур, используйте уникальные способности своей фракции,
                    и станьте величайшим полководцем в мире OneLife!
                  </p>
                </div>
                <div className="md:w-1/2">
                  <Image src="/gameplay.jpg" alt="OneLife Gameplay" width={600} height={400} className="rounded-lg shadow-lg" />
                </div>
              </div>
            </section>

            <section id="factions" className="py-20">
              <h2 className="text-4xl font-bold mb-6 text-center text-red-500">Фракции</h2>
              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                {factions.map((faction) => (
                  <FactionCard key={faction.name} {...faction} />
                ))}
              </div>
            </section>

            <section id="features" className="py-20">
              <h2 className="text-4xl font-bold mb-6 text-center text-red-500">Особенности игры</h2>
              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                {[
                  { icon: <Sword className="w-12 h-12" />, title: "Тактические сражения", description: "Каждая фигура имеет всего 1 единицу здоровья, делая каждое решение критически важным" },
                  { icon: <Shield className="w-12 h-12" />, title: "Смена дня и ночи", description: "Игровой процесс меняется в зависимости от времени суток, влияя на способности фигур" },
                  { icon: <Wind className="w-12 h-12" />, title: "Уникальные фракции", description: "Каждая фракция обладает своими уникальными механиками и картами" },
                  { icon: <Braces className="w-12 h-12" />, title: "Стратегическое управление", description: "Управляйте только двумя фигурами из шести за ход" },
                  { icon: "🛡", title: "Динамичное поле боя", description: "Используйте препятствия и захватывайте ключевые точки для победы" },
                  { icon: "💎", title: "Артефакты силы", description: "Сражайтесь за мощные артефакты, дающие уникальные преимущества" },
                ].map((feature, index) => (
                  <motion.div
                    key={index}
                    className="bg-gray-800 bg-opacity-50 p-6 rounded-lg"
                    whileHover={{ scale: 1.05 }}
                  >
                    <div className="text-4xl mb-4 text-red-500">{typeof feature.icon === 'string' ? feature.icon : feature.icon}</div>
                    <h3 className="text-xl font-semibold mb-2">{feature.title}</h3>
                    <p className="text-sm">{feature.description}</p>
                  </motion.div>
                ))}
              </div>
            </section>

            <section id="mechanics" className="py-20">
              <h2 className="text-4xl font-bold mb-6 text-center text-red-500">Механики игры</h2>
              <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div>
                  <h3 className="text-2xl font-semibold mb-4">Основные механики</h3>
                  <ul className="list-disc list-inside space-y-2">
                    <li>Карт-менеджмент и кубиковая атака</li>
                    <li>Смена дня и ночи, влияющая на игровой процесс</li>
                    <li>Уникальные способности фракций</li>
                    <li>Ограничение на количество действий за ход</li>
                    <li>Стратегическое использование препятствий</li>
                    <li>Погоня за мощным артефактом</li>
                  </ul>
                </div>
                <div>
                  <h3 className="text-2xl font-semibold mb-4">Игровое поле</h3>
                  <p className="mb-4">Гексагональное поле с 6 типами тайлов:</p>
                  <ul className="list-disc list-inside space-y-2">
                    <li>Флаг и лагерь - для расстановки юнитов и флага</li>
                    <li>Трава - стандартный тайл</li>
                    <li>Укрепление - +1 к урону/броне для юнита</li>
                    <li>Вода - разграничивает игроков</li>
                    <li>Ключевая точка - цель для победы</li>
                  </ul>
                </div>
              </div>
              <div className="mt-8">
                <h3 className="text-2xl font-semibold mb-4">Типы карт</h3>
                <p>В игре используются 4 типа карт:</p>
                <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mt-4">
                  {['Атака', 'Защита', 'Передвижение', 'Действие'].map((cardType) => (
                    <div key={cardType} className="bg-gray-800 bg-opacity-50 p-4 rounded-lg">
                      <h4 className="text-xl font-semibold mb-2">{cardType}</h4>
                      <p className="text-sm">Выполняет одноименную функцию в игре</p>
                    </div>
                  ))}
                </div>
              </div>
            </section>

            <section id="components" className="py-20">
              <h2 className="text-4xl font-bold mb-6 text-center text-red-500">Компоненты игры</h2>
              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                {[
                  { name: 'Игровые поля', count: 4 },
                  { name: 'Памятка по тайлам игрового поля', count: 1 },
                  { name: 'Табло день/ночь', count: 1 },
                  { name: 'Фракционные листы', count: 4 },
                  { name: 'Фишки юнитов', count: 24, note: '(в премиум версии: 3D модели)' },
                  { name: 'Фишки препятствий', count: 11 },
                  { name: 'Фишки флагов', count: 4 },
                  { name: 'Прочие фишки', count: 7 },
                  { name: 'Спас монеты', count: 4 },
                  { name: 'Игральные карты', count: 160, note: '(по 40 на каждую фракцию)' },
                  { name: 'Карты артефакта', count: 10 },
                  { name: 'Кубики D4', count: 4 },
                ].map((component, index) => (
                  <div key={index} className="bg-gray-800 bg-opacity-50 p-4 rounded-lg">
                    <h3 className="text-xl font-semibold mb-2">{component.name}</h3>
                    <p className="text-sm">Количество: {component.count}</p>
                    {component.note && <p className="text-sm text-gray-400">{component.note}</p>}
                  </div>
                ))}
              </div>
            </section>

            <section id="artwork" className="py-20">
              <h2 className="text-4xl font-bold mb-6 text-center text-red-500">Концепт-арт</h2>
              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <Image src="/concept-art-1.jpg" alt="Концепт-арт 1" width={400} height={300} className="rounded-lg shadow-lg" />
                <Image src="/concept-art-2.jpg" alt="Концепт-арт 2" width={400} height={300} className="rounded-lg shadow-lg" />
                <Image src="/concept-art-3.jpg" alt="Концепт-арт 3" width={400} height={300} className="rounded-lg shadow-lg" />
              </div>
            </section>

            <section id="video" className="py-20">
              <h2 className="text-4xl font-bold mb-6 text-center text-red-500">Видеогайд OneLife</h2>
              <div className="aspect-w-16 aspect-h-9">
                <iframe
                  src="https://www.youtube.com/embed/17Cx3yuCHH6BerbPiwakwhLEfCLBJK7ne"
                  frameBorder="0"
                  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                  allowFullScreen
                  className="w-full h-full rounded-lg shadow-lg"
                ></iframe>
              </div>
            </section>

            <section id="faq" className="py-20">
              <h2 className="text-4xl font-bold mb-6 text-center text-red-500">Часто задаваемые вопросы</h2>
              <Accordion type="single" collapsible className="w-full max-w-2xl mx-auto">
                <AccordionItem value="item-1" className="border-b border-gray-700">
                  <AccordionTrigger className="text-lg">Сколько человек могут играть в OneLife?</AccordionTrigger>
                  <AccordionContent className="text-gray-300">
                    OneLife предназначена для 2-4 игроков. Возможна как индивидуальная, так и командная игра.
                  </AccordionContent>
                </AccordionItem>
                <AccordionItem value="item-2" className="border-b border-gray-700">
                  <AccordionTrigger className="text-lg">Какова средняя продолжительность игры?</AccordionTrigger>
                  <AccordionContent className="text-gray-300">
                    Средняя продолжительность игры составляет 60-120 минут, в зависимости от количества игроков и их опыта.
                  </AccordionContent>
                </AccordionItem>
                <AccordionItem value="item-3" className="border-b border-gray-700">
                  <AccordionTrigger className="text-lg">Как работает механика дня и ночи?</AccordionTrigger>
                  <AccordionContent className="text-gray-300">
                    Игра чередует раунды дня и ночи, влияя на способности некоторых фигур и карт. Это добавляет дополнительный уровень стратегии и разнообразия в игровой процесс.
                  </AccordionContent>
                </AccordionItem>
                <AccordionItem value="item-4" className="border-b border-gray-700">
                  <AccordionTrigger className="text-lg">Что такое артефакты в игре?</AccordionTrigger>
                  <AccordionContent className="text-gray-300">
                    Артефакты - это мощные предметы на поле боя, которые можно захватить. Игрок, уничтоживший артефакт, получает случайный мощный бонус из колоды артефактов, что может значительно повлиять на ход игры.
                  </AccordionContent>
                </AccordionItem>
              </Accordion>
            </section>

            <section id="buy" className="py-20">
              <h2 className="text-4xl font-bold mb-6 text-center text-red-500">Где купить OneLife</h2>
              <div className="bg-gray-800 bg-opacity-50 p-8 rounded-lg max-w-2xl mx-auto">
                <p className="text-lg mb-4">
                  OneLife доступна для покупки в следующих магазинах:
                </p>
                <ul className="list-disc list-inside space-y-2 mb-6">
                  <li>Магазин настольных игр "Игромания"</li>
                  <li>Онлайн-магазин "Hobby Games"</li>
                  <li>Сеть магазинов "Мосигра"</li>
                </ul>
                <p className="text-lg mb-4">
                  Рекомендованная розничная цена: <span className="font-bold text-yellow-400">3999 рублей</span>
                </p>
                <Button className="w-full bg-red-600 hover:bg-red-700 text-xl transition-colors">Купить сейчас</Button>
              </div>
            </section>

            <section id="newsletter" className="py-20">
              <h2 className="text-4xl font-bold mb-6 text-center text-red-500">Подпишитесь на новости</h2>
              <div className="bg-gray-800 bg-opacity-50 p-8 rounded-lg max-w-2xl mx-auto">
                <p className="text-lg mb-4">
                  Будьте в курсе последних обновлений, турниров и специальных предложений!
                </p>
                <form onSubmit={handleSubmit} className="flex gap-4">
                  <Input
                    type="email"
                    placeholder="Ваш email"
                    className="flex-grow bg-gray-700 text-white"
                    value={email}
                    onChange={(e) => setEmail(e.target.value)}
                  />
                  <Button type="submit" className="bg-red-600 hover:bg-red-700 transition-colors">Подписаться</Button>
                </form>
                {subscribed && <p>Спасибо за подписку!</p>}
              </div>
            </section>
          </main>

          <footer className="bg-gray-900 py-8">
            <div className="container mx-auto px-4 text-center">
              <p>&copy; 2024 OneLife. Все права защищены.</p>
              <p className="mt-2">
                <Link href="https://vk.com/mindcraft_creative_studio" className="hover:underline text-red-500 transition-colors">
                  Следите за нами в ВКонтакте
                </Link>
              </p>
            </div>
          </footer>
        </div>
      </div>
    </>
  );
}


![Uploading 6caa12fcc78c4c71a644aa5581a85992.png…]()
![5](https://github.com/user-attachments/assets/fb4ae6ea-88a9-4002-a4d8-60391c8bc038)
![005a8b3b-45d4-4e78-b0fc-640add90f291](https://github.com/user-attachments/assets/24de5a0a-7c6d-4b2b-b43a-f3caaeb41c43)
![4e04e135-2661-4aaf-8224-1a07a712f649](https://github.com/user-attachments/assets/de25f5b6-9634-477b-8d1e-ba7d7606ebee)
![4a4d2351981b48d1ac3e8ff44143ffff](https://github.com/user-attachments/assets/39c086f3-4f10-4b93-9853-f2ec18e3c3bf)
![3d7e144c-f026-4f78-9025-17a6d1791f4e](https://github.com/user-attachments/assets/7c002b40-d965-4bdc-86f3-3684ca8d1974)
![3ce5afc11c1e45ac98ab729f8fcbf2e9 png~tplv-6bxrjdptv7-image](https://github.com/user-attachments/assets/e5784211-9c73-4d7e-b123-da18fdfc0d05)
![3a5O-shawaM](https://github.com/user-attachments/assets/d6d5b652-8448-4485-bcc1-e886cafdb29f)
![003d7b2f481e458aa77d69d29b8bceb3](https://github.com/user-attachments/assets/d2ebbf2d-7877-4376-8f5b-09aaa0a57945)
![2х2](https://github.com/user-attachments/assets/ef743274-f2dc-4a53-bd95-922472021174)
![2efd0cc8-52c6-429e-a764-59140c399ba6](https://github.com/user-attachments/assets/84fc49e5-2eef-4485-a705-11d24e53fc47)
![2e0e0d46932648d49a94072ad633b709](https://github.com/user-attachments/assets/fc247838-717c-4cab-93f2-f99c9eff839c)
![2b4e6e8c03bd49f7b65f4839fe55f7fa](https://github.com/user-attachments/assets/2422e3e3-fe86-4d11-9a02-92fad007e5df)
![02e9ce46-3b67-41a5-baca-ab3d5dfcee89](https://github.com/user-attachments/assets/485b2946-8ba8-46de-aee8-3b063e9424d4)
![1х1х1х1](https://github.com/user-attachments/assets/64cd5ce7-e802-456d-9fd7-a23005302d35)
![1х1х1х1- увеличенная](https://github.com/user-attachments/assets/953cb817-88be-4fee-8847-d637597d60bb)
![1х1х1](https://github.com/user-attachments/assets/116773b3-d318-4ae4-98a5-70b7ad979665)
![1х1х1- увеличенная](https://github.com/user-attachments/assets/1d10a463-ae48-4665-b19b-67cb5a2dc6b4)
![1х1](https://github.com/user-attachments/assets/b85e845b-916b-41b8-b64a-ef83baa4b718)
![1х1- увеличенная](https://github.com/user-attachments/assets/fd80a460-a987-4594-8245-d0d677271d8a)
![1ead9dc69a75086247d72aec2c71cf45_ck8kl514msb3id563j80_image_1](https://github.com/user-attachments/assets/b8e98b0c-5493-4289-9f83-00a922bd27b9)
![1c73ee86-fb96-4e8a-a86a-d712b6e71c75](https://github.com/user-attachments/assets/a3a57d22-7eaf-4cb8-a054-8401f558b7f7)
![00fb56befd4843f1a07e81bde5be81cd png~tplv-6bxrjdptv7-image](https://github.com/user-attachments/assets/4c01dd7e-f5be-449a-b56d-e70f2f3d6774)


git add .
git commit -m "Initial commit"
git push origin main

