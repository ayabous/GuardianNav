# 🛡️ Guardian
> **Assistant de sécurité intelligent à commande vocale**  
> "Parlez, on s'occupe du reste" - Système d'urgence avec IA Gemini, reconnaissance vocale française et géolocalisation.

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://python.org)
[![AI](https://img.shields.io/badge/AI-Google_Gemini_2.5_Flash-green.svg)](https://ai.google.dev/)
[![Voice](https://img.shields.io/badge/Voice-Vosk_French-orange.svg)](https://alphacephei.com/vosk/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 🎯 Vue d'ensemble

**Guardian** transforme votre voix en interface d'urgence intelligente. Dites simplement "Au secours, j'ai mal au cœur" et l'IA Gemini analyse votre situation, évalue l'urgence (1-10), guide votre réponse et déclenche automatiquement l'assistance appropriée (SMS famille, email avec carte, services d'urgence).

### ✨ Fonctionnalités principales
- 🎤 **Conversation naturelle** : Interface vocale française (Vosk + Google STT)
- 🤖 **IA contextuelle** : Analyse Gemini 2.5 Flash adaptée à chaque situation  
- ⚡ **Réponse < 7s** : Pipeline STT → IA → TTS → Actions optimisé
- 📱 **Notifications intelligentes** : SMS/Email personnalisés selon urgence
- 📍 **Géolocalisation précise** : GPS + What3Words + services d'urgence
- 🤸 **Détection automatique** : Chutes, immobilité, déviations GPS
- 👤 **Personnalisation complète** : Prénom, nom et numéro personnalisables
- 📧 **Emails d'urgence enrichis** : Localisation exacte et situation rapportée
- 💬 **Intégration WhatsApp** : Liens directs pour contacter la personne en danger
- 🧠 **Décision IA autonome** : L'agent décide intelligemment quand alerter les proches

---

## 🏗️ Architecture

### 🔄 Pipeline Conversation Vocale
```
🎤 Vosk STT (offline) → 🤖 Gemini Analysis → 🔊 Google TTS → 📱 Actions
   < 0.5s                    < 2s                < 1s        < 3s
```

### 🧠 Différence IA Classique vs Guardian
| Aspect | IA Classique | Guardian |
|--------|-------------|-------------|
| Interface | 📱 Boutons/Apps | 🗣️ Conversation naturelle |
| Logique | 📋 Règles figées | 🧠 Analyse contextuelle IA |
| Réponse | ⚙️ Générique | 🎯 Personnalisée situation |
| Déclenchement | 🚨 Manuel | 🎤 Vocal automatique |

### 🗂️ Structure Principale
```
guardian/
├── voice_conversation_agent.py    # Pipeline vocal principal
├── gemini_agent.py               # IA Gemini 2.5 Flash  
├── voice_agent.py                # STT multi-engine
├── speech_agent.py               # TTS contextuel
├── guardian_agent.py             # Orchestrateur urgences
├── sms_agent.py                  # Notifications Twilio
├── gmail_emergency_agent.py      # Emails d'urgence enrichis
├── emergency_response.py         # Emails + cartes
tests/                            # Tests organisés par catégorie
├── test_whatsapp.py             # Tests intégration WhatsApp
├── test_email_content.py        # Tests contenu emails
└── README.md                    # Documentation tests
run_tests.py                     # Runner de tests catégorisé
```

---

## 🚀 Installation

### 1️⃣ Clone & Setup
```bash
git clone https://github.com/organicanna/GuardianNav.git
cd GuardianNav
python3.9 -m venv .venv
source .venv/bin/activate  # macOS/Linux
pip install -r requirements.txt
```

### 2️⃣ Configuration APIs (api_keys.yaml)
```yaml
# IA Gemini (GRATUIT - ai.google.dev)
gemini:
  api_key: "VOTRE_CLE_GOOGLE_GENAI"

# Services Google Cloud  
google_cloud:
  text_to_speech:
    api_key: "VOTRE_CLE_TTS"
  maps:
    api_key: "VOTRE_CLE_GOOGLE_MAPS"

# Notifications Twilio
notification_services:
  twilio:
    account_sid: "ACXXXXXXX"
    auth_token: "VOTRE_TOKEN"
    phone_number: "+33123456789"

# Contacts urgence (avec WhatsApp)
emergency_contacts:
  - name: "Marie Dupont" 
    phone: "+33612345678"
    email: "marie@gmail.com"
    relation: "fille"
    priority: 1

# Configuration Gmail pour emails d'urgence
gmail:
  enabled: true
  email: "votre.email@gmail.com"
  app_password: "VOTRE_MOT_DE_PASSE_APP"
```

### 3️⃣ Test Installation
```bash
# Test APIs
python debug_gemini.py

# Test audio  
python -c "import sounddevice as sd; print(sd.query_devices())"
```

---

## 💡 Utilisation

### 🎭 Mode Démo (Recommandé)
```bash
python demo_live_agent.py
# Scénario personnalisable: saisissez votre prénom, nom et numéro
# Démonstration avec vraie reconnaissance vocale et IA Gemini
# - Interface personnalisée avec votre identité
# - Emails d'urgence avec vos informations réelles
# - Liens WhatsApp directs vers votre numéro
# - Décision intelligente d'alerte par l'IA
```

### 🛡️ Mode Production  
```bash
python main.py
# Surveillance complète avec détection automatique
```

### 🗣️ Exemples Conversation

#### Urgence Médicale
```
👤 "J'ai mal à la poitrine, ça serre fort"
🤖 "URGENCE CARDIAQUE ! Asseyez-vous ! J'appelle le SAMU."
🧠 IA évalue: Niveau 9/10 → Alerte automatique des proches
📱 Actions: SAMU contacté + Email d'urgence avec localisation exacte + Liens WhatsApp
```

#### Navigation
```  
👤 "Je suis perdu dans le métro"
🤖 "Vous êtes à Châtelet. Quelle est votre destination ?"
👤 "République"
🤖 "Prenez la ligne 1 direction Vincennes, 3 stations."
```

#### Sécurité
```
👤 "Quelqu'un me suit depuis 10 minutes"  
🤖 "Dirigez-vous vers le commissariat à 200m. J'alerte vos proches."
🧠 IA évalue: Danger réel détecté → Envoi automatique d'email d'urgence
📍 Actions: Localisation exacte partagée + Refuges sûrs + WhatsApp pour contact direct
```

### 🎤 Commandes Vocales
- **Urgences** : "Au secours", "J'ai mal à...", "Je suis en danger"  
- **Info** : "Où suis-je ?", "Hôpital le plus proche"
- **Navigation** : "Comment rentrer ?", "Je suis perdu"
- **Test** : "Test du système", "Ma position"

---

## 🧪 Tests & Validation

### 🏃‍♂️ Runner de Tests Catégorisé
```bash
# Tests par catégorie
python run_tests.py email      # Tests emails et WhatsApp
python run_tests.py ai         # Tests IA et analyse
python run_tests.py voice      # Tests reconnaissance vocale  
python run_tests.py security   # Tests sécurité
python run_tests.py config     # Tests configuration

# Tous les tests
python run_tests.py all
```

### 📧 Fonctionnalités Testées
- ✅ **Intégration WhatsApp** : Génération liens, messages pré-remplis
- ✅ **Contenu emails** : Localisation réelle, situation rapportée  
- ✅ **Décision IA** : Évaluation automatique du niveau d'urgence
- ✅ **Personnalisation** : Noms, numéros, contacts personnalisés

### 📖 Documentation Tests
```bash
# Voir la documentation complète
cat tests/README.md
```

---

## ⚙️ Configuration

### 🎤 Reconnaissance Vocale (Vosk)
```python
vosk_config = {
    "model_path": "vosk-model-small-fr-0.22",  # Français offline
    "confidence_threshold": 0.7,               # 70% minimum
    "sample_rate": 16000                       # Optimisé français
}
```

### 🤖 IA Gemini Personnalisée
```yaml
# Décision intelligente d'alerte automatique
ai_decision_config:
  auto_alert_threshold: 7      # Seuil auto-envoi emails (sur 10)
  danger_keywords: ["suivie", "menacée", "agressée", "blessée"]
  emergency_contexts: ["nuit", "isolé", "danger immédiat"]

# Mots-clés urgence personnalisés  
emergency_keywords:
  critical: ["au secours", "samu", "infarctus"]     # Niveau 9-10  
  high: ["j'ai très mal", "je suis tombé"]          # Niveau 7-8
  medium: ["je ne me sens pas bien", "j'ai peur"]   # Niveau 4-6
```

### 🔊 Synthèse Vocale Adaptative
```python
# TTS selon urgence
emergency_voice = {
    "speaking_rate": 1.2,    # +20% rapide
    "pitch": "+3st",         # Aigu attention
    "volume_gain_db": 6.0    # Plus fort
}
```

### 📧 Emails d'Urgence Enrichis
```html
<!-- Contenu automatique des emails -->
✅ Localisation exacte : "8 rue de Londres, 75009 Paris (bureaux Google France)"
✅ Situation rapportée : Texte exact de reconnaissance vocale
✅ Liens WhatsApp : "Appeler [Nom] via WhatsApp" → Clic direct
✅ Actions immédiates : Boutons d'aide et instructions
✅ Carte interactive : Localisation précise sur Google Maps
```

### 💬 Intégration WhatsApp
```javascript
// Génération automatique de liens WhatsApp
const whatsappLink = `https://wa.me/${phoneNumber}?text=${prefilledMessage}`;
// Message pré-rempli en français rassurant
// Un clic depuis l'email = appel direct gratuit
```

---

## 📊 Performance

| Métrique | Temps | Technologie |
|----------|-------|-------------|
| 🎤 Reconnaissance | < 0.5s | Vosk offline français |
| 🤖 Analyse IA + Décision | < 2s | Gemini 2.5 Flash |
| 🔊 Synthèse vocale | < 1s | Google TTS Neural |
| 📱 Notifications + WhatsApp | < 3s | Gmail + Twilio |
| 📧 Email enrichi + Carte | < 2s | Gmail API + Maps |
| **🎯 Total** | **< 7s** | **Bout en bout** |

### 🆕 Nouvelles Capacités
- ✅ **Personnalisation temps réel** : Prénom/nom saisis → Interface adaptée
- ✅ **Décision IA autonome** : Évaluation 1-10 → Alerte automatique si > 7
- ✅ **WhatsApp instantané** : Email → Clic → Appel gratuit en 1 seconde
- ✅ **Localisation exacte** : GPS + adresse lisible dans tous les emails

---

## � Dernières Améliorations (v2.0)

### 👤 **Personnalisation Complète**
- Interface s'adapte au prénom/nom saisi en temps réel
- Messages d'accueil personnalisés : "Bonjour [Prénom]"
- Emails d'urgence avec l'identité réelle de l'utilisateur

### 🧠 **IA Décisionnelle Autonome**  
- L'agent évalue automatiquement la gravité (1-10/10)
- Envoi automatique d'emails pour situations dangereuses (≥ 7/10)
- Analyse contextuelle : nuit + isolé + mots-clés danger = alerte immédiate

### 📧 **Emails d'Urgence Nouvelle Génération**
- Localisation exacte : "8 rue de Londres, 75009 Paris (Google France)"
- Situation rapportée mot pour mot depuis la reconnaissance vocale
- Boutons WhatsApp : 1 clic = appel direct gratuit
- Interface HTML responsive avec cartes interactives

### 🧪 **Infrastructure de Tests Robuste**
- Tests organisés par catégorie (email, IA, vocal, sécurité)
- Runner automatisé avec rapports détaillés
- Validation continue de l'intégration WhatsApp et emails

---

## �🆘 Urgence & Support

**En cas d'urgence réelle : appelez le 15 (SAMU), 17 (Police), 18 (Pompiers)**

---

## 📄 Licence

MIT License - Voir [LICENSE](LICENSE)

---

> **🛡️ Guardian - Votre sécurité par la voix**  
> *Développé avec ❤️ pour protéger ce qui compte le plus*
