import { useEffect, useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Heart, Gift, Music } from "lucide-react";
import { motion } from "framer-motion";

export default function ProposalPage() {
  const [countdown, setCountdown] = useState(10);
  const [showProposal, setShowProposal] = useState(false);
  const [showResponse, setShowResponse] = useState(false);
  const [accepted, setAccepted] = useState(false);
  const [noButtonOffset, setNoButtonOffset] = useState(0);
  const [showGift, setShowGift] = useState(false);
  const [musicPlaying, setMusicPlaying] = useState(true);
  const romanticMusic = typeof Audio !== "undefined" ? new Audio("/romantic-music.mp3") : null;

  useEffect(() => {
    const audio = new Audio("/countdown-music.mp3");
    audio.loop = true;
    audio.volume = 0.5;
    audio.play();

    if (countdown > 0) {
      const timer = setTimeout(() => setCountdown(countdown - 1), 1000);
      return () => clearTimeout(timer);
    } else {
      audio.pause();
      setShowProposal(true);
    }
  }, [countdown]);

  useEffect(() => {
    if (accepted) {
      if (romanticMusic) {
        romanticMusic.loop = true;
        romanticMusic.play();
      }
      setTimeout(() => setShowGift(true), 3000);
    }
  }, [accepted]);

  const handleNoClick = () => {
    setNoButtonOffset(noButtonOffset + 1);
  };

  const toggleMusic = () => {
    if (!romanticMusic) return;
    if (musicPlaying) {
      romanticMusic.pause();
    } else {
      romanticMusic.play();
    }
    setMusicPlaying(!musicPlaying);
  };

  const photoUrls = Array.from({ length: 10 }, (_, i) => `/pookie${i + 1}.jpg`);
  const videoUrls = ["/pookie-video1.mp4", "/pookie-video2.mp4"];

  return (
    <div className="min-h-screen bg-gradient-to-br from-pink-100 to-pink-200 flex flex-col items-center justify-center p-4 relative overflow-hidden">
      {/* Dreamy glow */}
      <div className="absolute inset-0 bg-[radial-gradient(ellipse_at_center,_var(--tw-gradient-stops))] from-pink-300/30 via-transparent to-transparent blur-3xl z-0" />

      {/* Background Falling Flowers Animation */}
      <div className="absolute inset-0 z-0 overflow-hidden pointer-events-none">
        {Array.from({ length: 30 }).map((_, i) => (
          <motion.div
            key={i}
            className="absolute text-pink-400 text-2xl"
            style={{ top: `${Math.random() * 100}%`, left: `${Math.random() * 100}%` }}
            animate={{ y: "100vh" }}
            transition={{ duration: 10 + Math.random() * 10, repeat: Infinity, ease: "linear" }}
          >
            🌸
          </motion.div>
        ))}
      </div>

      {/* Sparkles */}
      <div className="absolute inset-0 z-0 pointer-events-none">
        {Array.from({ length: 40 }).map((_, i) => (
          <div
            key={i}
            className="absolute w-1 h-1 bg-white rounded-full opacity-70 animate-ping"
            style={{ top: `${Math.random() * 100}%`, left: `${Math.random() * 100}%`, animationDuration: `${1 + Math.random() * 2}s` }}
          ></div>
        ))}
      </div>

      {/* Fireworks */}
      <div className="absolute inset-0 z-0 pointer-events-none">
        {Array.from({ length: 10 }).map((_, i) => (
          <div
            key={i}
            className="absolute w-2 h-2 bg-yellow-300 rounded-full animate-ping"
            style={{ top: `${Math.random() * 100}%`, left: `${Math.random() * 100}%`, animationDuration: `${0.5 + Math.random()}s`, animationDelay: `${Math.random()}s` }}
          ></div>
        ))}
      </div>

      {/* Flying Doves */}
      <div className="absolute inset-0 pointer-events-none z-0">
        {Array.from({ length: 4 }).map((_, i) => (
          <motion.img
            key={i}
            src="/dove.png"
            alt="Dove"
            className="w-12 h-12 absolute opacity-60"
            initial={{ x: -100, y: Math.random() * 500 }}
            animate={{ x: '110%' }}
            transition={{ duration: 8 + Math.random() * 4, repeat: Infinity, ease: "linear", delay: i }}
          />
        ))}
      </div>

      {/* Rotating Ring */}
      {!accepted && (
        <motion.div
          className="absolute top-10 right-10 w-20 h-20 z-10"
          animate={{ rotate: 360 }}
          transition={{ repeat: Infinity, duration: 4, ease: "linear" }}
        >
          <img src="/ring.png" alt="Ring" className="w-full h-full" />
        </motion.div>
      )}

      {!showProposal && (
        <>
          <motion.h1
            className="text-5xl font-bold text-pink-600 mb-8 text-center z-10"
            initial={{ opacity: 0, y: -50 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 1 }}
          >
            A Special Surprise Awaits...
          </motion.h1>
          <motion.div
            className="text-3xl font-semibold text-gray-700 z-10"
            initial={{ scale: 0 }}
            animate={{ scale: 1 }}
            transition={{ duration: 1 }}
          >
            Countdown: {countdown}
          </motion.div>
        </>
      )}

      {showProposal && !showResponse && (
        <motion.div
          className="max-w-xl text-center z-10"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ delay: 0.5, duration: 1 }}
        >
          <h1 className="text-5xl font-bold text-pink-600 mb-6">
            Pookie, Will You Marry Me? 💍
          </h1>
          <p className="text-lg text-gray-700 italic mb-4">
            "Zindagi bhar ke safar mein, har pal tujhe chaahta rahun — Pookie, tu meri hamesha rahegi kya?"
          </p>
          <Button
            className="text-xl px-6 py-3 bg-pink-500 text-white rounded-full mt-4 shadow-md hover:bg-pink-600"
            onClick={() => setShowResponse(true)}
          >
            Respond Now
          </Button>
        </motion.div>
      )}

      {showResponse && !accepted && (
        <motion.div
          className="flex gap-6 mt-10 z-10 relative"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ duration: 0.8 }}
        >
          <Button
            className="text-xl px-6 py-3 bg-green-500 text-white rounded-full shadow-md hover:bg-green-600"
            onClick={() => setAccepted(true)}
          >
            Yes ❤️
          </Button>
          <motion.div
            animate={{ x: `${noButtonOffset * 50}px`, y: `${noButtonOffset * 20}px` }}
            transition={{ type: "spring", stiffness: 100 }}
            className="absolute"
          >
            <Button
              className="text-xl px-6 py-3 bg-gray-300 text-gray-800 rounded-full shadow-md"
              onClick={handleNoClick}
            >
              No 😅
            </Button>
          </motion.div>
        </motion.div>
      )}

      {accepted && (
        <motion.div
          className="mt-10 text-center z-10"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ duration: 1 }}
        >
          <h2 className="text-4xl font-bold text-pink-600 mb-4">She said YES! 💖</h2>
          <p className="text-lg text-gray-700 italic mb-6">
            "Ab har subah teri muskurahat se shuru hogi, aur har raat teri baahon mein khatam."
          </p>
          <div className="mb-4">
            <Button onClick={toggleMusic} className="flex items-center gap-2 bg-pink-300 text-white px-4 py-2 rounded-full shadow-md hover:bg-pink-400">
              <Music className="w-4 h-4" /> {musicPlaying ? "Pause Music" : "Play Music"}
            </Button>
          </div>
          <div className="w-full max-w-3xl mx-auto">
            <motion.div
              className="grid grid-cols-2 gap-4 mb-4"
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              transition={{ duration: 1 }}
            >
              {photoUrls.map((url, i) => (
                <img key={i} src={url} alt={`Pookie ${i + 1}`} className="rounded-2xl shadow-lg" />
              ))}
              {videoUrls.map((src, i) => (
                <video key={i} controls className="rounded-2xl shadow-lg col-span-2">
                  <source src={src} type="video/mp4" />
                </video>
              ))}
            </motion.div>
          </div>
          {showGift && (
            <motion.div
              className="mt-8 text-center"
              initial={{ scale: 0 }}
              animate={{ scale: 1 }}
              transition={{ duration: 0.5 }}
            >
              <div className="inline-flex flex-col items-center">
                <Gift className="w-12 h-12 text-yellow-500 animate-bounce" />
                <p className="mt-2 text-pink-600 font-semibold">A surprise is waiting for you... 🎁</p>
              </div>
            </motion.div>
          )}
        </motion.div>
      )}

      <div className="absolute bottom-10 flex gap-2 animate-pulse z-10">
        {Array.from({ length: 10 }).map((_, i) => (
          <Heart key={i} className="text-red-400 w-6 h-6 animate-bounce" />
        ))}
      </div>
    </div>
  );
}
