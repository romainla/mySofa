# Methodes de calculs et solveurs

<EulerImplicit> <EulerExplicit> : indique la façon dont sera solutionne le probleme (x(t + dt) = f(x(t+dt)) dans le premier cas, x(t + dt) = f(x(t)) dans le second cas) mais ne calcule pas la solution -> il faut un solveur (dans le C++ les EulerImplicit et Explicit prépare les matrices A et b du système Ax=b mais c'est tout)

<CGLinearSolver> : solutionne le problème Ax=b de manière itérative, on lui précise le nombre d'itérations max et les seuils de tolérance

# Description physique de l'objet

<MeshGmshLoader> : se charge juste d'ouvrir le fichier mais ne crée aucun objet -> si on s'en contente y a rien qui s'affiche

<MechanicalObject> : liste les degrés de liberté de l'objet ainsi que les points qui le constituent (sans savoir comment ils sont reliés entre eux). C'est sur lui que sera appliqué le solveur pour savoir les effets des forces

<TopologyContainer> : c'est lui qui indique comment les points du MechanicalObject sont liés entre eux (FEM, mass-spring etc)

<RegularGridSpringForceField> : forces internes d'un mass-spring model

# Description visuelle

<Mesh> : permet de charger le mash pour le rendering

Bonne pratique : creer un noeud enfant qui va se charger du mapping visuel<->physique

<BarycentricMapping input="@.." output="@Visual">: les deformations que le MechanicalObject vont subir seront repercutees sur le rendu visuel.

# Types de masse

<UniformMass>: tous les points ont la meme masses et basta -> pas realiste du tout

<DiagonalMass>: prend en compte la topologie de l'objet (e.g si c'est des triangles, des tetraedres) et met les masses sur les sommets en tenant compte de cette topologie (e.g un triangle plus grand aura des masses plus importantes sur les noeuds qu'un triangle petit) -> un peu plus realiste mais pas top

<MeshMatrixMass>: reparti la masse a l'interieur des elements finis: majorite de la masse aux sommets puis va en decroissant lorsque l'on s'en eloigne

# Forces

<PlaneForceField>: forces exterieures a l'objet qui s'appliquent a lui

<FEMForceField>: ils en existent des différents selon la topologie de l'objet (e.g HexahedralFEMForceField pour des FEM utilisant des hexaedres). Ils exercent les forces internes propres au modele que l'on decrit (necessitent un TopologyContainer) ->permettent la definition du module de Young et du coef de Poisson pour les objets deformables

# Collisions

Bonne pratique: creer un noeud enfant qui va se charger du mapping collision<->physique
<MeshLoader>: charge le mesh qui va etre utilise pour le calcul de collision. Il peut etre different du mesh visuel (e.g generalement mesh visuel plus detaille que mesh collision pour avoir un beau rendu visuel mais allege les calculs)

<BarycentricMapping>: permet le mapping avec le MechanicalObject pour qu'il subisse des forces lorsqu'il y a collision avec un autre objet et donc pour la mise a jour de sa position. Si les deux mesh ne sont pas identiques, il va calculer les barycentres de surfaces, volumes etc du mesh le plus detaille pour le mapper au mesh le moins detaille
