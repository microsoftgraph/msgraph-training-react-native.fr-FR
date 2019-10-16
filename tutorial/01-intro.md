<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d5c2f-101">Ce didacticiel vous apprend à créer une application REACT native qui utilise l’API Microsoft Graph pour récupérer des informations de calendrier pour un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d5c2f-101">This tutorial teaches you how to build an React Native app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="d5c2f-102">Si vous préférez télécharger simplement le didacticiel terminé, vous pouvez télécharger ou cloner le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-react-native).</span><span class="sxs-lookup"><span data-stu-id="d5c2f-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-react-native).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5c2f-103">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="d5c2f-103">Prerequisites</span></span>

<span data-ttu-id="d5c2f-104">Avant de commencer ce didacticiel, les éléments suivants doivent être installés sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="d5c2f-104">Before you start this tutorial, you should have the following installed on your development machine.</span></span>

- <span data-ttu-id="d5c2f-105">Au moins l’un des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="d5c2f-105">At least one of the following:</span></span>
  - <span data-ttu-id="d5c2f-106">Kit de développement [Android Studio](https://developer.android.com/studio/) **et** [java](https://jdk.java.net) (requis pour exécuter l’exemple sur Android)</span><span class="sxs-lookup"><span data-stu-id="d5c2f-106">[Android Studio](https://developer.android.com/studio/) **and** [Java Development Kit](https://jdk.java.net) (required to run the sample on Android)</span></span>
  - <span data-ttu-id="d5c2f-107">[Xcode](https://developer.apple.com/xcode/) (requis pour exécuter l’exemple sur iOS)</span><span class="sxs-lookup"><span data-stu-id="d5c2f-107">[Xcode](https://developer.apple.com/xcode/) (required to run the sample on iOS)</span></span>
- [<span data-ttu-id="d5c2f-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="d5c2f-108">Node.js</span></span>](https://nodejs.org)

> [!NOTE]
> <span data-ttu-id="d5c2f-109">Ce didacticiel a été écrit à l’aide de l’interface CLI native de REACT, qui présente des conditions préalables spécifiques en fonction de votre système d’exploitation et des plateformes cibles.</span><span class="sxs-lookup"><span data-stu-id="d5c2f-109">This tutorial was written using the React Native CLI, which has specific prerequisites depending on your operating system and target platforms.</span></span> <span data-ttu-id="d5c2f-110">Pour obtenir des instructions sur la configuration de votre ordinateur de développement, voir [REACT Native Getting Started](https://facebook.github.io/react-native/docs/getting-started) .</span><span class="sxs-lookup"><span data-stu-id="d5c2f-110">See [React Native Getting Started](https://facebook.github.io/react-native/docs/getting-started) for instructions on configuring your development machine.</span></span> <span data-ttu-id="d5c2f-111">Les versions utilisées pour ce didacticiel sont répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="d5c2f-111">The versions used for this tutorial are listed below.</span></span> <span data-ttu-id="d5c2f-112">Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.</span><span class="sxs-lookup"><span data-stu-id="d5c2f-112">The steps in this guide may work with other versions, but that has not been tested.</span></span>
>
> - <span data-ttu-id="d5c2f-113">Android Studio version 3.4.2 avec 1.8.0 JRE et le kit de développement logiciel (SDK) Android 9,0</span><span class="sxs-lookup"><span data-stu-id="d5c2f-113">Android Studio version 3.4.2 with the 1.8.0 JRE and the Android 9.0 SDK</span></span>
> - <span data-ttu-id="d5c2f-114">Kit de développement Java version 12.0.2</span><span class="sxs-lookup"><span data-stu-id="d5c2f-114">Java Development Kit version 12.0.2</span></span>
> - <span data-ttu-id="d5c2f-115">Xcode version 10,3</span><span class="sxs-lookup"><span data-stu-id="d5c2f-115">Xcode version 10.3</span></span>
> - <span data-ttu-id="d5c2f-116">Node. js version 10.16.0</span><span class="sxs-lookup"><span data-stu-id="d5c2f-116">Node.js version 10.16.0</span></span>

## <a name="feedback"></a><span data-ttu-id="d5c2f-117">Commentaires</span><span class="sxs-lookup"><span data-stu-id="d5c2f-117">Feedback</span></span>

<span data-ttu-id="d5c2f-118">Veuillez fournir des commentaires sur ce didacticiel dans le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-react-native).</span><span class="sxs-lookup"><span data-stu-id="d5c2f-118">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-react-native).</span></span>
