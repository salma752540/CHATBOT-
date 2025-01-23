<<<<<<< HEAD
# Implémentation d'un Chatbot Basé sur LangChain


### Introduction
Ce document présente le rapport technique pour l’implémentation d’un chatbot utilisant la bibliothèque LangChain. Le chatbot est conçu pour répondre à des questions en exploitant des données contextuelles stockées dans un vecteurstore FAISS et un modèle préentraîné à base de langage. L’objectif principal est de fournir des réponses précises et concises, tout en assurant une gestion adaptée des éventuelles inconnues.


### Architecture du Chatbot

#### 1. **Modules Utilisés**
- **LangChain Community** : Pour le chargement des documents (à partir de fichiers PDF) et la gestion des embeddings.
- **FAISS** : Système de stockage vectoriel pour les recherches efficaces de similarité.
- **CTransformers** : Pour intégrer le modèle de langage Llama 2.
- **Chainlit** : Interface utilisateur interactive permettant la gestion des échanges avec les utilisateurs.

#### 2. **Pipeline Fonctionnel**
Le chatbot suit les étapes suivantes :
1. **Chargement des Embeddings** : Utilisation de « sentence-transformers/all-MiniLM-L6-v2 » pour représenter les données en embeddings vectoriels.
2. **Chargement du Modèle** : Modèle Llama 2 préentraîné chargé via la bibliothèque CTransformers.
3. **Initialisation du Prompt** : Modèle de prompt personnalisé pour fournir des réponses optimales et éviter des réponses non fiables.
4. **Recherche Contextuelle** : Utilisation de FAISS comme vecteurstore pour rechercher les contextes les plus pertinents.
5. **Interaction avec l’Utilisateur** :
   - Gestion asynchrone via Chainlit.
   - Formatage et présentation des réponses, incluant les sources si disponibles.


### Fonctionnalités Détaillées

#### 1. **Gestion du Prompt Personnalisé**
Le prompt est conçu pour limiter les réponses non fiables. Voici le modèle utilisé :
```text
Use the following pieces of information to answer the user's question.
If you don't know the answer, just say that you don't know, don't try to make up an answer.

Context: {context}
Question: {question}

Only return the helpful answer below and nothing else.
Helpful answer:
```

#### 2. **Intégration de FAISS**
FAISS permet d’indexer et de rechercher efficacement les documents à partir de leurs embeddings vectoriels. Dans le code, la base FAISS est initialisée avec le chemin suivant :
```
DB_FAISS_PATH = 'vectorstores/db_faiss'
```

#### 3. **Modèle de Langage**
Le modèle **Llama 2-7B** est utilisé, avec les paramètres suivants :
- **Nombre maximal de tokens** : 512.
- **Température** : 0.5 (pour équilibrer la diversité et la précision des réponses).

#### 4. **Gestion des Réponses et des Sources**
Le chatbot retourne les réponses au format suivant :
- **Question** : Rappel de la question posée.
- **Réponse** : Réponse générée par le modèle.
- **Source** : Liste des documents consultés avec les numéros de page.
En cas d’absence de sources, un message spécifique est présenté.

#### 5. **Fonctionnalités Asynchrones**
L’interaction utilisateur est gérée de manière asynchrone pour garantir une fluidité optimale via Chainlit. Cela inclut :
- Détection des réponses répétées.
- Mise à jour des messages sans interrompre la session utilisateur.

### **Flexibilité et Réutilisabilité du Chatbot**

Notre chatbot est conçu pour être générique et adaptable grâce à sa structure modulaire. Voici les points qui mettent en avant cette réutilisabilité :

- **Indépendance des données :**

Le modèle est entraîné à partir d'embeddings vectoriels générés à partir des documents fournis (PDF ou autres formats).
Ces embeddings capturent la signification et le contexte des données, permettant au chatbot de s'adapter à tout type de contenu.
- **Modularité de la Solution :**

* Chargement des données : Vous pouvez remplacer les documents médicaux actuels par des données d'autres domaines (par exemple, droit, finance, éducation).
* FAISS et Qdrant : La base de données vectorielle peut être recréée pour chaque ensemble de données, garantissant une recherche rapide et pertinente dans n'importe quel domaine.
Modèle Llama 2 Préentraîné :

Le modèle Llama 2 utilisé est déjà généraliste. En modifiant simplement le contexte fourni dans les données, il peut répondre à des questions sur divers sujets.
- **Exemple de Réutilisation :**

Aujourd’hui, notre chatbot répond à des questions médicales sur le cancer du cerveau.
Demain, en chargeant des données juridiques (par exemple, des lois et des règlements), il pourrait devenir un assistant juridique pour aider les utilisateurs à interpréter les lois.
- **Facilité de Mise en Œuvre :**

Il suffit de :
* Collecter les nouveaux documents pertinents.
* Générer des embeddings à partir de ces documents.
* Réinitialiser la base de données vectorielle FAISS/Qdrant.
* Déployer le chatbot avec ces nouvelles données.
### **Avantages pour les Utilisateurs :**


Économique : Un seul framework pour plusieurs domaines.

Rapide : Adaptation quasi-instantanée aux nouvelles données.

Efficace : Répond aux besoins spécifiques sans avoir à recréer un nouveau modèle.


---

### Conclusion
Le chatbot implémenté est une solution robuste pour gérer des requêtes basées sur un contexte documentaire. En exploitant LangChain, FAISS et un modèle Llama 2, il fournit des réponses pertinentes tout en maintenant une expérience utilisateur adaptée. Par exemple, ce chatbot pourrait être utilisé dans le domaine médical pour répondre à des questions spécifiques à partir de publications scientifiques, ou dans les entreprises pour fournir une assistance interne basée sur leurs bases de données documentaires. Avec quelques améliorations, cette solution pourrait être déployée à grande échelle pour divers cas d’utilisation.

=======


# Llama2 Medical Bot

The Llama2 Medical Bot is a powerful tool designed to provide medical information by answering user queries using state-of-the-art language models and vector stores. This README will guide you through the setup and usage of the Llama2 Medical Bot.

## Table of Contents

- [Introduction](#langchain-medical-bot)
- [Table of Contents](#table-of-contents)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

Before you can start using the Llama2 Medical Bot, make sure you have the following prerequisites installed on your system:

- Python 3.6 or higher
- Required Python packages (you can install them using pip):
    - langchain
    - chainlit
    - sentence-transformers
    - faiss
    - PyPDF2 (for PDF document loading)

## Installation

1. Clone this repository to your local machine.

    ```bash
    git clone https://github.com/your-username/langchain-medical-bot.git
    cd langchain-medical-bot
    ```

2. Create a Python virtual environment (optional but recommended):

    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use: venv\Scripts\activate
    ```

3. Install the required Python packages:

    ```bash
    pip install -r requirements.txt
    ```

4. Download the required language models and data. Please refer to the Langchain documentation for specific instructions on how to download and set up the language model and vector store.

5. Set up the necessary paths and configurations in your project, including the `DB_FAISS_PATH` variable and other configurations as per your needs.

## Getting Started

To get started with the Llama2 Medical Bot, you need to:

1. Set up your environment and install the required packages as described in the Installation section.

2. Configure your project by updating the `DB_FAISS_PATH` variable and any other custom configurations in the code.

3. Prepare the language model and data as per the Langchain documentation.

4. Start the bot by running the provided Python script or integrating it into your application.

## Usage

The Llama2 Medical Bot can be used for answering medical-related queries. To use the bot, you can follow these steps:

1. Start the bot by running your application or using the provided Python script.

2. Send a medical-related query to the bot.

3. The bot will provide a response based on the information available in its database.

4. If sources are found, they will be provided alongside the answer.

5. The bot can be customized to return specific information based on the query and context provided.

## Contributing

Contributions to the Llama2 Medical Bot are welcome! If you'd like to contribute to the project, please follow these steps:

1. Fork the repository to your own GitHub account.

2. Create a new branch for your feature or bug fix.

3. Make your changes and ensure that the code passes all tests.

4. Create a pull request to the main repository, explaining your changes and improvements.

5. Your pull request will be reviewed, and if approved, it will be merged into the main codebase.

## License

This project is licensed under the MIT License.

---

For more information on how to use, configure, and extend the Llama2 Medical Bot, please refer to the Langchain documentation or contact the project maintainers.

Happy coding with Llama2 Medical Bot! 🚀
>>>>>>> b37f3012 (Premier commit : ajout des fichiers)
