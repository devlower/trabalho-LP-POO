package com.example.gestao_locadora

import java.time.LocalDate
import java.time.format.DateTimeFormatter
import java.lang.Exception
import java.lang.IndexOutOfBoundsException
import kotlin.system.exitProcess

val allMedias: MutableList<Medias> = mutableListOf()
val allClients: MutableList<Clients> = mutableListOf()
val allStaff: MutableList<Staff> = mutableListOf()
val allRents: MutableList<Rent> = mutableListOf()
val manager: StorageControl = StorageControl()

fun main() {
    val initMenu = MenuPrincipal()
    initMenu.menus()

}

class MenuPrincipal {

    fun menus() {
        var userInput = 0
        while (userInput != 1 && userInput != 2 && userInput != 3 && userInput != 4 && userInput != 5 && userInput != 6 && userInput != 7 && userInput != 8 && userInput != 9) {
            Menu.values().forEach { println(it.description) }
            try {
                userInput = readLine()?.toInt() ?: 0
            } catch (e: Exception) {
                continue
            }
        }
        when (userInput) {
            1 -> {
                manager.rentMedia()
                println("\nPressione qualquer tecla para prosseguir...")
                readLine()
                menus()
            }
            2 -> {
                manager.returnMedia()
                println("\nPressione qualquer tecla para prosseguir...")
                readLine()
                menus()
            }
            3 -> {
                manager.createNewMedia()
                println("\nMídia cadastrada com sucesso...")
                readLine()
                menus()
            }
            4 -> {
                var option: Int? = null
                do {
                    allMedias.sortWith(compareBy { it.name })
                    allMedias.forEachIndexed { index, medias ->

                        println("${index + 1} Título = ${medias.name}, ${medias.qt}\n")
                    }
                    println("\nEscolha uma mídia (pelo seu número) para editar")
                    try {
                        option = readLine()?.toInt()?.minus(1)
                        print("\nMídia selecionada: ${allMedias[option!!].name}\nQuantidade atual de exemplares: ${allMedias[option].qt}\n\n")
                    } catch (ex: Exception) {
                        when (ex) {
                            is IndexOutOfBoundsException -> {
                                continue
                            }
                        }
                        continue
                    }
                } while (option == null || option > allMedias.size || option < 0)
                var opt = 0
                do {
                    println("Selecionar tipo de alteração\n")
                    println("1 - ADICIONAR exemplar")
                    println("2 - DELETAR exemplar")
                    try {
                        opt = readLine()!!.toInt()
                    } catch (e: Exception) {
                        continue
                    }
                } while (opt != 1 && opt != 2)
                when (opt) {
                    1 -> {
                        do {
                            var qrAdd = 0
                            println("Qual a quantidade de exemplares você deseja adicionar ao estoque de: ${allMedias[option].name}")
                            try {
                                qrAdd = readLine()!!.toInt()
                            } catch (e: Exception) {
                                continue
                            }
                            if (qrAdd > 0) {
                                allMedias[option].qt += qrAdd
                                println("A quantidade atual de exemplares para o título ${allMedias[option].name} é ${allMedias[option].qt}")
                            }
                        } while (qrAdd == 0 || qrAdd < 0)
                    }
                    2 -> {
                        do {
                            var qrAdd = 0
                            println("Qual a quantidade de exemplares você deseja deletar do estoque de: ${allMedias[option].name}")
                            try {
                                qrAdd = readLine()!!.toInt()
                            } catch (e: Exception) {
                                continue
                            }
                            if (qrAdd > 0) {
                                allMedias[option].qt -= qrAdd
                                println("A quantidade atual de exemplares para o título ${allMedias[option].name} é ${allMedias[option].qt}")
                            }
                        } while (qrAdd == 0 || qrAdd < 0)
                    }
                }
                println("\nPressione qualquer tecla para prosseguir...")
                readLine()
                menus()
            }
            5 -> {
                manager.createNewClient()
                println("\nCliente cadastrado com sucesso...\n")
                println("\nPressione qualquer tecla para prosseguir...")
                readLine()
                menus()
            }
            6 -> {
                manager.createNewStaff()
                println("\n Funcionario(a) cadastrado com sucesso...")
                println("\nPressione qualquer tecla para prosseguir...")
                readLine()
                menus()
            }
            7 -> {
                manager.bonus()
                println("\nPressione qualquer tecla para prosseguir...")
                readLine()
                menus()
            }
            8 -> {
                userInput = 0
                while (userInput != 1 && userInput != 2 && userInput != 3 && userInput != 4 && userInput != 5) {
                    MenuList.values().forEach { println(it.description) }
                    try {
                        userInput = readLine()?.toInt() ?: 0
                    } catch (e: Exception) {
                        continue
                    }
                }
                when (userInput) {
                    1 -> {
                        allMedias.sortWith(compareBy { it.name })
                        allMedias.forEach { println(it.toString()) }
                        println("\nPressione qualquer tecla para prosseguir...")
                        readLine()
                        menus()
                    }
                    2 -> {
                        allStaff.sortWith(compareBy { it.name })
                        allStaff.forEach { println(it.toString()) }
                        println("\nPressione qualquer tecla para prosseguir...")
                        readLine()
                        menus()
                    }
                    3 -> {
                        allClients.sortWith(compareBy { it.name })
                        allClients.forEach { println(it.toString()) }
                        println("\nPressione qualquer tecla para prosseguir...")
                        readLine()
                        menus()
                    }
                    4 -> {
                        allRents.forEach { println(it.toString()) }
                        println("\nPressione qualquer tecla para prosseguir...")
                        readLine()
                        menus()
                    }
                    5 -> menus()
                    6 -> exitProcess(0)
                }

            }
            9 -> exitProcess(0)
        }
    }

    enum class Menu(val description: String) {
        TITLE("Gerenciamento de Locadora:\n"),
        FIRST("1 - Alugar Mídia"),
        SECOND("2 - Devolução Mídia"),
        THIRD("3 - Cadastrar Mídia"),
        FOURTH("4 - Atualizar Estoque"),
        FIFTH("5 - Cadastrar Cliente"),
        SIXTH("6 - Cadastrar Funcionario(a)"),
        SEVENTH("7 - Bonificar Funcionario(a)"),
        EIGHTH("8 - Listar"),
        NINETH("9 - Encerrar Programa")
    }

    enum class MenuList(val description: String) {
        TITLE("Opções de Listagem:\n"),
        FIRST("1 - Listar Midia"),
        SECOND("2 - Listar Funcionarios(as)"),
        THIRD("3 - Listar Clientes"),
        FOURTH("4 - Listar Alugueis"),
        FIFTH("5 - Voltar ao Menu Principal"),
        SIXTH("6 - Encerrar Programa")
    }
}

open class Person(var name: String, var surname: String, val cpf: String) {
    override fun toString(): String {
        return "Nome = $name $surname\nCPF = $cpf\n"
    }
}

class Clients(name: String, surname: String, cpf: String) : Person(name, surname, cpf)
class Staff(
    name: String,
    surname: String,
    cpf: String,
    var bonus: Double = 0.0,
    var selling: Int = 0
) : Person(name, surname, cpf) {
    override fun toString(): String {

        return super.toString() + "Bonus = R$ " + "%.2f".format(bonus) + "\nLocações = $selling\n"
    }
}

class Medias(
    var name: String,
    val director: String,
    val year: String,
    val description: String,
    var type: String,
    var qt: Int
) {
    override fun toString(): String {
        return "Título = $name\nDiretor = $director\nAno de lançamento = $year\nDescrição = $description\nTipo de mídia = $type\nAcervo = $qt\n"
    }
}

class StorageControl {
    init {
        // initializing DVD media
        val mid1 = Medias(
            "Donnie Darko",
            "Richard Kelly",
            "2001",
            "Donnie (Jake Gyllenhaal) é um jovem brilhante e excêntrico, que cursa o colegial mas despreza a grande maioria dos seus colegas de escola. Donnie tem visões, em especial de um coelho monstruoso o qual apenas ele consegue ver, que o encorajam a realizar brincadeiras destrutivas e humilhantes com quem o cerca. Até que um dia uma de suas visões o atrai para fora de casa e lhe diz que o mundo acabará dentro de um mês. Donnie inicialmente não acredita na profecia, mas momentos depois um avião cai bem no telhado de sua casa, quase matando-o. É quando ele começa a se perguntar qual o fundo de verdade da sua previsão.",
            "DVD",
            5
        )
        val mid2 = Medias(
            "Inception",
            "Christopher Nolan",
            "2010",
            "Em um mundo onde é possível entrar na mente humana, Cobb (Leonardo DiCaprio) está entre os melhores na arte de roubar segredos valiosos do inconsciente, durante o estado de sono. Além disto ele é um fugitivo, pois está impedido de retornar aos Estados Unidos devido à morte de Mal (Marion Cotillard). Desesperado para rever seus filhos, Cobb aceita a ousada missão proposta por Saito (Ken Watanabe), um empresário japonês: entrar na mente de Richard Fischer (Cillian Murphy), o herdeiro de um império econômico, e plantar a ideia de desmembrá-lo. Para realizar este feito ele conta com a ajuda do parceiro Arthur (Joseph Gordon-Levitt), a inexperiente arquiteta de sonhos Ariadne (Ellen Page) e Eames (Tom Hardy), que consegue se disfarçar de forma precisa no mundo dos sonhos.",
            "DVD",
            13
        )
        val mid3 = Medias(
            "The Shining",
            "Stanley Kubrick",
            "1980",
            "Durante o inverno, um homem (Jack Nicholson) é contratado para ficar como vigia em um hotel no Colorado e vai para lá com a mulher (Shelley Duvall) e seu filho (Danny Lloyd). Porém, o contínuo isolamento começa a lhe causar problemas mentais sérios e ele vai se tornado cada vez mais agressivo e perigoso, ao mesmo tempo em que seu filho passa a ter visões de acontecimentos ocorridos no passado, que também foram causados pelo isolamento excessivo.",
            "DVD",
            11
        )
        val mid4 = Medias(
            "Star Wars: The Phantom Menace",
            "George Lucas",
            "1999",
            "Em Star Wars: Episódio I, quando a maquiavélica Federação Comercial planeja invadir o pacífico planeta Naboo, o guerreiro Jedi Qui-Gon Jinn (Liam Neeson) e seu aprendiz Obi-Wan Kenobi (Ewan McGregor) embarcam em uma aventura para tentar salvar o planeta. Viajam com eles a jovem Rainha Amidala (Natalie Portman), que é visada pela Federação pois querem forçá-la a assinar um tratado político. Eles têm de viajar para os distantes planetas Tatooine e Coruscant em uma desesperada tentativa de salvar o mundo de Darth Sidious (Ian McDiarmid), o demoníaco líder da Federação que sempre surge em imagens tridimensionais (a ameaça fantasma). Durante a viagem, Qui-Gon Jinn conhece um garoto de nove anos que deseja treiná-lo para ser tornar um Jedi, pois o menino tem todas as qualidades para isto. Mas o tempo revelará que nem sempre as coisas são o que aparentam.",
            "DVD",
            6
        )
        val mid5 = Medias(
            "Star Wars: Attack of the Clones",
            "George Lucas",
            "2002",
            "Em Star Wars: Episódio 2, dez anos após a tentativa frustrada de invasão do planeta Naboo, Obi-Wan Kenobi (Ewan McGregor), Anakin Skywalker (Hayden Christensen) e Padmé Amidala (Natalie Portman) estão juntos novamente. Neste período de tempo Obi-Wan passou de aprendiz a professor dos ensinamentos jedi para Anakin, sendo que ambos foram destacados para proteger a agora senadora Amidala, que tem sua vida ameaçada por facções separatistas da República, que ameaçam desencadear uma guerra civil intergalática. Com o passar do tempo surge um romance proibido entre Anakin e Amidala, pois os cavaleiros jedi não têm permissão para se apaixonarem.",
            "DVD",
            6
        )
        val mid6 = Medias(
            "Star Wars: Revenge of the Sith",
            "George Lucas",
            "2005",
            "Em Star Wars: Episódio III, as Guerras Clônicas estão em pleno andamento e as diferenças entre o Conselho Jedi e o Chanceler Palpatine (Ian McDiarmid) aumentam cada vez mais. Anakin Skywalker (Hayden Christensen) mantém um elo de lealdade com Palpatine, ao mesmo tempo em que luta para que seu casamento com Padmé Amidala (Natalie Portman) não seja afetado por esta situação. Seduzido por promessas de poder, Anakin se aproxima cada vez mais de Darth Sidious (Ian McDiarmid) até se tornar o temível Darth Vader. Juntos eles tramam um plano para aniquilar de uma vez por todas com os cavaleiros jedi.",
            "DVD",
            6
        )
        val mid7 = Medias(
            "Star Wars: The Force Awakens",
            "J.J. Abrams",
            "2015",
            "Em Star Wars: Episódio VII, décadas após a queda de Darth Vader e do Império, surge uma nova ameaça: a Primeira Ordem, uma organização sombria que busca minar o poder da República e que tem Kylo Ren (Adam Driver), o General Hux (Domhnall Gleeson) e o Líder Supremo Snoke (Andy Serkis) como principais expoentes. Eles conseguem capturar Poe Dameron (Oscar Isaac), um dos principais pilotos da Resistência, que antes de ser preso envia através do pequeno robô BB-8 o mapa de onde vive o mitológico Luke Skywalker (Mark Hamill). Ao fugir pelo deserto, BB-8 encontra a jovem Rey (Daisy Ridley), que vive sozinha catando destroços de naves antigas. Paralelamente, Poe recebe a ajuda de Finn (John Boyega), um stormtrooper que decide abandonar o posto repentinamente. Juntos, eles escapam do domínio da Primeira Ordem.",
            "DVD",
            6
        )
        val mid8 = Medias(
            "Star Wars: The Last Jedi",
            "Rian Johnson",
            "2017",
            "Em Star Wars: Episódio VIII, após encontrar o mítico e recluso Luke Skywalker (Mark Hamill) em uma ilha isolada, a jovem Rey (Daisy Ridley) busca entender o balanço da Força a partir dos ensinamentos do Mestre Jedi. Paralelamente, a Primeira Ordem de Kylo Ren (Adam Driver) se reorganiza para enfrentar a Resistência.",
            "DVD",
            6
        )
        val mid9 = Medias(
            "Star Wars: A New Hope",
            "George Lucas",
            "1978",
            "Em Star Wars - Episódio IV, o jovem Luke Skywalker (Mark Hamill) sonha ir para a Academia como seus amigos, mas se vê envolvido em uma guerra intergalática quando seu tio compra dois robôs e com eles encontra uma mensagem da princesa Leia Organa (Carrie Fisher) para o Jedi Obi-Wan Kenobi (Alec Guinness) sobre os planos da construção da Estrela da Morte, uma gigantesca estação espacial com capacidade para destruir um planeta. Luke então se junta aos cavaleiros Jedi e a Han Solo (Harrison Ford), um mercenário, para tentar destruir esta terrível ameaça ao lado dos membros da resistência.",
            "DVD",
            6
        )
        val mid10 = Medias(
            "Star Wars: Return of the Jedi",
            "Richard Marquand",
            "1983",
            "Em Star Wars: Episódio VI, o Imperador Palpatine (Ian McDiarmid) está supervisionando a construção de uma nova Estrela da Morte. Enquanto isso, Luke Skywalker (Mark Hamill) liberta Han Solo (Harrison Ford) e a Princesa Leia (Carrie Fisher) das mãos de Jabba, o pior bandido das galáxias. Luke só se tornará um cavaleiro Jedi quando destruir Darth Vader, que ainda pretende atraí-lo para o lado negro da \"Força\". No entanto a luta entre os dois vai revelar um inesperado segredo.",
            "DVD",
            6
        )
        allMedias.add(mid1)
        allMedias.add(mid2)
        allMedias.add(mid3)
        allMedias.add(mid4)
        allMedias.add(mid5)
        allMedias.add(mid6)
        allMedias.add(mid7)
        allMedias.add(mid8)
        allMedias.add(mid9)
        allMedias.add(mid10)

        // initializing video games
        val mid11 = Medias(
            "Crash Bandicoot",
            "Naughty Dog",
            "1996",
            "Em Crash Bandicoot, Crash é capturado por Dr Neo Cortex e seu assistente N. Brio, durante seu cativeiro Crash conhece uma bandicoot fêmea chamada Tawna. N. Brio cria uma máquina conhecida como Evolvo-ray que dá inteligencia para os animais, enquanto Cortex cria Vortice de Cortex para submeter os animais a serem escravos, ele usaria a máquina em Crash para  transforma-lo no general de seu exercito do mal, porém ela ainda estava incompleta e por mais que N. Brio tenha advertido, Cortex optou por ativa-la em Crash, que tinha muita bondade em sí e não pode ser controlado para o mal. Ele foi considerado um erro e jogado para fora do laboratório, assim deixando Tawna para trás. N. Sanity foi a ilha onde Crash foi parar, nessa mesma ilha ele conhece Aku Aku, um antigo espirito que agora vive em uma mascara de madeira. Então os dois partem em uma aventura por várias ilhas até retornar ao laborátorio onde Tawna está presa, passando por uma aldeia indígena liderada por Papu Papu, um templo onde se encontram com um cachorro chamado Ripper Roo, uma mina vulcânica onde encontram Koala Kong, assim chegando à ilha final, cruzando a Estação de Poder de Cortex encontram e derrotam Pinstripe Potoroo, ao chegar no castelo encontram e derrotam o criador do Envolvo-raio, N. Brio. Crash vai para o dirigivel de Cortex que sobrevoa o castelo, Cortex é derrutado pela inteligencia de Crash que rebate todos os seus raios de plasma o derrubando de sua pequena nave. Por fim Crash salva Tawna e os dois vão embora no dirigivel.",
            "GAME",
            12
        )
        val mid12 = Medias(
            "Hitman: Absolution",
            "IO Interactive",
            "2012",
            "Hitman: Absolution não está definido para ocorrer durante a cronologia da série. Detalhes da trama confirmados incluem o Agente 47, o protagonista da série, a assassinar Diana Burnwood, ex-funcionária da Agência Internacional de Contratos, e o antagonista identificado como Blake Dexter, um novo personagem na série. O personagem foi dublado por Keith Carradine.",
            "GAME",
            16
        )
        val mid13 = Medias(
            "The Last of Us",
            "Naughty Dog",
            "2013",
            "Após o sucesso da série Uncharted, a Naughty Dog retorna ao PlayStation 3 com The Last of Us, um game que mistura o mundo pós-apocalíptico de “Eu Sou a Lenda” com dilemas humanos de “The Walking Dead” em uma sociedade à beira do caos. Nele, encontramos Joel e Ellie, uma dupla improvável que se une para tentar sobreviver à loucura que o mundo se tornou após um epidemia ter transformado toda a população em monstros. Porém, mais do que enfrentar essas criaturas, os protagonistas também terão de lidar com outros grupos de sobreviventes, que usarão várias táticas para emboscar e roubar os personagens. Em um universo no qual a moral e a ética não têm mais espaço, é cada um por si.",
            "GAME",
            16
        )
        val mid14 = Medias(
            "Gris",
            "Nomada",
            "2018",
            "Gris é uma esperançosa jovem perdida em seu próprio mundo, lidando com uma experiência dolorosa em sua vida. Sua jornada através da tristeza se manifesta em seu vestido, que concede novas habilidades para melhor navegar sua realidade desbotada. Conforme a história se desenrola, Gris vai crescer emocionalmente e ver seu mundo de uma maneira diferente, revelando novos caminhos para explorar usando suas novas habilidades.",
            "GAME",
            5
        )
        val mid15 = Medias(
            "Donkey Kong",
            "Rare",
            "1994",
            "Em Donkey Kong Country, Donkey Kong, ao lado de seu sobrinho e companheiro inseparável Diddy Kong, devem recuperar as bananas roubadas por King K. Rool e seus capangas, os Kremlings. Após verificar o vazio esconderijo de bananas, localizado logo abaixo de sua casa em Kongo Jungle, Donkey Kong embarca em uma aventura em sua terra nativa, a Ilha Donkey Kong. Enquanto coleta as bananas nas diferentes regiões da ilha, Donkey Kong deve derrotar vários inimigos, incluindo os repteis Kremlings, além de outras perigosas criaturas nativas da ilha. Ajudando-o em sua missão estão alguns dos outros Kongs da família: Diddy acompanha Donkey Kong em sua aventura, Cranky providencia dicas (e alívio cômico), Candy opera os pontos de gravação ao redor da ilha, e Funky oferece meio de transporte. Também apoiando Donkey Kong em certas ocasiões estão vários 'colegas animais' (o rinoceronte Rambi, a avestruz Expresso, o peixe-espada Enguarde, o sapo Winky, e o papagaio Squawks), cada um com habilidades diferentes. Após superar as diferentes áreas da ilha, Donkey Kong finalmente chega ao navio pirata Gangplank Galleon, onde o arquirrival de Donkey Kong e líder dos Kremlings, King K. Rool, o aguarda. Após derrotá-lo, o jogo termina, com a cena final mostrando o esconderijo de bananas de Donkey Kong em toda sua antiga glória, cheia de bananas novamente.",
            "GAME",
            9
        )
        allMedias.add(mid11)
        allMedias.add(mid12)
        allMedias.add(mid13)
        allMedias.add(mid14)
        allMedias.add(mid15)

        // initializing books
        val mid16 = Medias(
            "The Perks of Being a Wallflower",
            "Stephen Chbosky",
            "2007",
            "Em As vantagens de ser invisível, Charlie conta suas descobertas nos primeiros anos do ensino médio. A narrativa é toda desenvolvida por meio de cartas que Charlie envia para um destinatário desconhecido, o jovem conta a sua rotina, dos amigos e fala acerca da relação com a família. No início do livro uma tragédia provoca uma grande ruptura na vida de Charlie. Seu melhor amigo comete suicídio e isso balança as estruturas do garoto e dos demais alunos. Entretanto, este não é o episódio que causou o impacto mais intenso na vida de Charlie, pois ele já era considerado uma pessoa diferente há algum tempo, o que deixa claro que existe algo mais profundo.",
            "LIVRO",
            12
        )
        val mid17 = Medias(
            "The Lord of the Rings: The Fellowship of the Ring",
            "J. R. R. Tolkien",
            "1954",
            "Numa cidadezinha indolente do Condado, um jovem hobbit é encarregado de uma imensa tarefa. Deve empreender uma perigosa viagem através da Terra-média até as Fendas da Perdição, e lá destruir o Anel do Poder - a única coisa que impede o domínio maléfico do Senhor do Escuro. A Sociedade do Anel é a primeira parte da grande obra de ficção fantástica de J. R. R. Tolkien, O Senhor dos Anéis. É impossível transmitir ao novo leitor todas as qualidades e o alcance do livro. Alternadamente cômica, singela, épica, monstruosa e diabólica, a narrativa desenvolve-se em meio a inúmeras mudanças de cenários e de personagens, num mundo imaginário absolutamente convincente em seus detalhes. Nas palavras do romancista Richard Hughes, “quanto à amplitude imaginativa, a obra praticamente não tem paralelos e é quase igualmente notável na sua vividez e na habilidade narrativa, que mantêm o leitor preso página após página”. Tolkien criou em O Senhor dos Anéis uma nova mitologia num mundo inventado que demonstrou possuir um poder de atração atemporal.",
            "LIVRO",
            4
        )
        val mid18 = Medias(
            "The Lord of the Rings: The Two Towers",
            "J. R. R. Tolkien",
            "1954",
            "O segundo volume de O Senhor dos Anéis, mais importante épico de fantasia moderno, narra os caminhos separados seguidos pelos membros da Sociedade do Anel em sua luta para deter Sauron, o Senhor Sombrio da terra de Mordor, e destruir o Um Anel, no qual está contida a maior parte do poder do tirano demoníaco imaginado por J.R.R. Tolkien.",
            "LIVRO",
            9
        )
        val mid19 = Medias(
            "The Lord of the Rings: The Return of the King",
            "J. R. R. Tolkien",
            "1955",
            "O Retorno do Rei A guerra entre os Povos Livres da Terra-média e Sauron, o Senhor Sombrio da terra de Mordor, chega a seu clímax neste terceiro volume do romance O Senhor dos Anéis. As batalhas grandiosas que estão prestes a acontecer, no entanto, são apenas o pano de fundo para o verdadeiro drama: a missão quase suicida dos hobbits Frodo e Sam, que tentam destruir o Um Anel, fonte do poder de Sauron, infiltrando-se no coração do território do Inimigo. Em O Retorno do Rei, acompanhamos o mago Gandalf e o hobbit Pippin em sua visita à a majestosa cidade de Minas Tirith, que já foi o principal baluarte dos Homens contra a ameaça de Sauron, mas que está prestes a sucumbir diante da força avassaladora do Senhor Sombrio. Outros membros da Sociedade do Anel se juntam a Aragorn, herdeiro da longa linhagem dos reis de Minas Tirith, na tentativa de evitar que a antiga capital do reino de Gondor seja destruída. Nas fronteiras de Mordor, Sam resgata Frodo, e os dois hobbits partem para o último estágio de sua jornada rumo ao Monte da Perdição, uma jornada que testará os limites do corpo e da mente dos pequenos heróis. O livro inclui ainda numerosos apêndices, nos quais Tolkien explora detalhes da história, das línguas, dos alfabetos e até dos calendários de seu mundo ficcional.",
            "LIVRO",
            16
        )
        val mid20 = Medias(
            "Uzumaki",
            "Junji Ito",
            "1999",
            "Kirie Goshima e Shuichi Saito vivem na pequena cidade de Kurôzu-cho, que se vê repentinamente assolada por uma estranha maldição: os seus habitantes tornam-se obcecados por objetos com a forma de espirais (conchas de caracol, remoinhos e padrões) e acabam por morrer misteriosamente. Kirie and Shuichi elaboram um plano para escapar da cidade, mas os seus esforços não têm sucesso e no regresso acabam por descobrir qual é afinal o centro da espiral.",
            "LIVRO",
            5
        )
        allMedias.add(mid16)
        allMedias.add(mid17)
        allMedias.add(mid18)
        allMedias.add(mid19)
        allMedias.add(mid20)

        // initializing clients
        val client1 = Clients("João Marcelo", "Danza", "123.456.789-01")
        val client2 = Clients("Adriana Aparecida", "Assenço", "321.654.987.02")
        val client3 = Clients("Leonardo Helder", "Moraes", "123.654.789-03")
        val client4 = Clients("Leonilda", "Riceto", "321.456.987-04")
        val client5 = Clients("Gilmara", "Maravilha", "147.852.369-05")
        allClients.add(client1)
        allClients.add(client2)
        allClients.add(client3)
        allClients.add(client4)
        allClients.add(client5)

        // initializing staff
        val staff1 = Staff("Hanna", "Parreira", "741.258.963-06")
        val staff2 = Staff("Angelica", "dos Santos", "963.258.741-07")
        val staff3 = Staff("Gustavo", "Broglio", "987.456.321-08")
        val staff4 = Staff("Eduarda", "Pádua", "951.753.654-09")
        val staff5 = Staff("Lucas", "Omar", "852.951.753-10")
        allStaff.add(staff1)
        allStaff.add(staff2)
        allStaff.add(staff3)
        allStaff.add(staff4)
        allStaff.add(staff5)

        // initializing rents
        allStaff.sortWith(compareBy { it.name })
        allClients.sortWith(compareBy { it.name })
        allMedias.sortWith(compareBy { it.name })
        val rent1 =
            Rent(
                "${allStaff[0].name} ${allStaff[0].surname}",
                "${allClients[3].name} ${allClients[3].surname}",
                arrayOf(allMedias[13].name, allMedias[16].name).toMutableList(),
                arrayOf(allMedias[13].type, allMedias[16].type).toMutableList(),
                "09-06-2022",
                "ALUGADO",
                "Título ainda não devolvido",
                10.00,
                0.0

            )
        allStaff[0].selling += rent1.mediaRent.size
        val rent2 =
            Rent(
                "${allStaff[0].name} ${allStaff[0].surname}",
                "${allClients[0].name} ${allClients[0].surname}",
                arrayOf(allMedias[6].name).toMutableList(),
                arrayOf(allMedias[6].type).toMutableList(),
                "13-06-2022",
                "ALUGADO",
                "Título ainda não devolvido",
                5.00,
                0.0
            )
        allStaff[0].selling += rent2.mediaRent.size
        val rent3 =
            Rent(
                "${allStaff[3].name} ${allStaff[3].surname}",
                "${allClients[2].name} ${allClients[2].surname}",
                listOf(allMedias[15].name).toMutableList(),
                arrayOf(allMedias[15].type).toMutableList(),
                "15-06-2022",
                "ALUGADO",
                "Título ainda não devolvido",
                5.00,
                0.0
            )
        allStaff[3].selling += rent3.mediaRent.size
        val rent4 =
            Rent(
                "${allStaff[1].name} ${allStaff[1].surname}",
                "${allClients[4].name} ${allClients[4].surname}",
                arrayOf(allMedias[15].name).toMutableList(),
                arrayOf(allMedias[15].type).toMutableList(),
                "15-06-2022",
                "ALUGADO",
                "Título ainda não devolvido",
                5.00,
                0.0
            )
        allStaff[1].selling += rent4.mediaRent.size
        val rent5 =
            Rent(
                "${allStaff[2].name} ${allStaff[2].surname}",
                "${allClients[1].name} ${allClients[1].surname}",
                arrayOf(allMedias[17].name).toMutableList(),
                arrayOf(allMedias[17].type).toMutableList(),
                "20-06-2022",
                "ALUGADO",
                "Título ainda não devolvido",
                5.00,
                0.0
            )
        allStaff[2].selling += rent5.mediaRent.size
        allRents.add(rent1)
        allRents.add(rent2)
        allRents.add(rent3)
        allRents.add(rent4)
        allRents.add(rent5)
    }

    fun createNewMedia() {
        var title = ""
        var director = ""
        var ano = ""
        var description = ""
        var type = ""
        var qt = 0

        print("Insira o nome do novo título: ")
        readLine()?.let { title = it }
        print("Insira o nome do diretor do título: ")
        readLine()?.let { director = it }
        print("Insira o ano de lançamento do título: ")
        readLine()?.let { ano = it }
        print("Insira descrição do título: ")
        readLine()?.let { description = it }
        var userInput = 0
        while (userInput != 1 && userInput != 2 && userInput != 3) {
            println("Insira o tipo de mídia: ")
            println("1 - DVD")
            println("2 - GAME")
            println("3 - LIVRO")
            try {
                userInput = readLine()?.toInt() ?: 0
            } catch (e: Exception) {
                continue
            }
        }
        when (userInput) {
            1 -> type = "DVD"
            2 -> type = "GAME"
            3 -> type = "LIVRO"
        }
        do {
            var check = false
            print("Insira a quantidade adquirida: ")
            try {
                readLine()?.let {
                    qt = it.toInt()
                    check = true
                }
            } catch (e: Exception) {
                check = false
                continue
            }
        } while (!check)

        allMedias.add(Medias(title, director, ano, description, type, qt))
    }

    fun createNewClient() {
        var name = ""
        var surname = ""
        var cpf = ""

        print("Insira o nome do cliente: ")
        readLine()?.let { name = it }
        print("Insira o sobrenome do client: ")
        readLine()?.let { surname = it }
        print("Insira o CPF do cliente: ")
        readLine()?.let { cpf = it }

        allClients.add(Clients(name, surname, cpf))
    }

    fun createNewStaff() {
        var name = ""
        var surname = ""
        var cpf = ""

        print("Insira o primeiro nome do Funcionario(a): ")
        readLine()?.let { name = it }
        print("Insira o sobrenome do Funcionario(a): ")
        readLine()?.let { surname = it }
        print("Insira o CPF do Funcionario(a): ")
        readLine()?.let { cpf = it }

        allStaff.add(Staff(name, surname, cpf))
    }

    fun rentMedia() {
        val colabName: String
        val clientName: String
        val mediaRent = emptyArray<String>().toMutableList()
        val mediaType = emptyArray<String>().toMutableList()
        val dateOfRent: String
        val status: String
        val colabReturn = ""
        val price: Double
        val multa: Double

        allMedias.sortWith(compareBy { it.name })
        allStaff.sortWith(compareBy { it.name })
        allClients.sortWith(compareBy { it.name })
        var optionStaff: Int? = null
        do {
            println("Selecione (pelo seu número) o Funcionario(a) responsável: \n")
            allStaff.forEachIndexed { index, staff ->
                println("${index + 1} - Funcionario(a): ${staff.name} ${staff.surname}")
            }
            try {
                optionStaff = readLine()?.toInt()?.minus(1)
                print("\nFuncionario(a) selecionado: ${allStaff[optionStaff!!].name} ${allStaff[optionStaff].surname}\n\n")
            } catch (ex: Exception) {
                when (ex) {
                    is IndexOutOfBoundsException -> {
                        continue
                    }
                }
                continue
            }
        } while (optionStaff == null || optionStaff > allStaff.size || optionStaff < 0)
        colabName = "${allStaff[optionStaff].name} ${allStaff[optionStaff].surname}"

        var option: Int? = null
        do {
            println("Selecione (pelo seu número) o cliente: \n")
            allClients.forEachIndexed { index, client ->
                println("${index + 1} - Cliente: ${client.name} ${client.surname}")
            }
            try {
                option = readLine()?.toInt()?.minus(1)
                print("\nCliente selecionado: ${allClients[option!!].name} ${allClients[option].surname}\n\n")
            } catch (ex: Exception) {
                when (ex) {
                    is IndexOutOfBoundsException -> {
                        continue
                    }
                }
                continue
            }
        } while (option == null || option > allClients.size || option < 0)
        clientName = "${allClients[option].name} ${allClients[option].surname}"

        option = null
        var addMore: Int? = null
        do {
            println("Selecione o título a ser alugado: \n")
            allMedias.forEachIndexed { index, medias ->
                println("${index + 1} - Título: ${medias.name}")
            }
            try {
                option = readLine()?.toInt()?.minus(1)
                print("\nTítulo selecionado: ${allMedias[option!!].name}\n\n")
            } catch (ex: Exception) {
                when (ex) {
                    is IndexOutOfBoundsException -> {
                        continue
                    }
                }
                continue
            }
            mediaRent.add(allMedias[option].name)
            mediaType.add(allMedias[option].type)
            println("Deseja inserir mais um título?")
            println("1 - Sim")
            println("2 - Não")
            try {
                addMore = readLine()?.toInt()!!
            } catch (ex: Exception) {
                when (ex) {
                    is IndexOutOfBoundsException -> {
                        continue
                    }
                }
                continue
            }
            if (addMore == 2)
                break
        } while (option == null || option > allClients.size || option < 0 || addMore == 1)

        allStaff[optionStaff].selling += mediaRent.size

        dateOfRent = LocalDate.now().format(DateTimeFormatter.ofPattern("dd-MM-yyyy"))
        status = "ALUGADO"
        if (mediaRent.size >= 5) {
            val fullPrice = (mediaRent.size.toDouble() * 5)
            price = fullPrice * 0.9
            println(
                "\nPromoção de 10% aplicada a locação - 5 ou mais títulos alugados\n\nPreço: R$ " + "%.2f".format(
                    price
                )
            )
        } else {
            price = mediaRent.size.toDouble() * 5
            println("\nPreço da locação: R$ " + "%.2f".format(price))
        }
        multa = 0.0


        allRents.add(
            Rent(
                colabName,
                clientName,
                mediaRent,
                mediaType,
                dateOfRent,
                status,
                colabReturn,
                price,
                multa
            )
        )

    }

    fun returnMedia() {
        var optionStaff: Int? = null
        do {
            println("Selecione (pelo seu número) o Funcionario(a) responsável: \n")
            allStaff.forEachIndexed { index, staff ->
                println("${index + 1} - Funcionario(a): ${staff.name} ${staff.surname}")
            }
            try {
                optionStaff = readLine()?.toInt()?.minus(1)
                print("\nFuncionario(a) selecionado: ${allStaff[optionStaff!!].name} ${allStaff[optionStaff].surname}\n\n")
            } catch (ex: Exception) {
                when (ex) {
                    is IndexOutOfBoundsException -> {
                        continue
                    }
                }
                continue
            }
        } while (optionStaff == null || optionStaff > allStaff.size || optionStaff < 0)

        var option: Int? = null
        do {
            println("Selecione (pelo seu número) o Título a ser devolvido: \n")
            allRents.forEachIndexed { index, rent ->
                println(
                    "${index + 1} - Título: \n- ${rent.mediaRent.joinToString(" \n- ")}\nCliente: ${rent.clientName}\nData do aluguel: ${rent.dateOfRent}\nMáximo dia de retorno: ${
                        rent.maxDayReturn(
                            rent.dateOfRent
                        )
                    }\nFuncionario(a) responsável pelo aluguel: ${rent.colabName}\nStatus: ${rent.status}\n"
                )
            }
            try {
                option = readLine()?.toInt()?.minus(1)
                allRents[option!!].status = "DEVOLVIDO"
                print("\nTítulo Selecionado: \n- ${allRents[option].mediaRent.joinToString("\n- ")}\nStatus: ${allRents[option].status}\n\n")
            } catch (ex: Exception) {
                when (ex) {
                    is IndexOutOfBoundsException -> {
                        continue
                    }
                }
                continue
            }
        } while (option == null || option > allStaff.size || option < 0)
        val convertDtOfRent =
            LocalDate.parse(
                allRents[option].maxDayReturn(allRents[option].dateOfRent),
                DateTimeFormatter.ofPattern("dd-MM-yyyy")
            )
        val dtNow = LocalDate.now()
        val blnDt = dtNow.isAfter(convertDtOfRent)
        if (blnDt) {
            allRents[option].multa = allRents[option].price * 0.5
            println(
                "\nTítulo devolvido fora do prazo estabelecido\n\n\t- Aplicar multa:\n\t\tCliente: ${allRents[option].clientName}\n\t\tData do aluguel: ${allRents[option].dateOfRent}\n\t\tData máxima de retorno: ${
                    allRents[option].maxDayReturn(
                        allRents[option].dateOfRent
                    )
                }\n\t\tMulta: 50% do valor alugado\n\t\tValor da multa: R$ " + "%.2f".format(
                    allRents[option].multa
                )
            )
        }
        allRents[option].colabReturn =
            "${allStaff[optionStaff].name} ${allStaff[optionStaff].surname}"
    }

    fun bonus() {
        allStaff.sortWith(compareBy { it.selling })
        println("\nAtribuindo bonus de R$ 200.00 ao funcionario(a) que obteve o maior número de locações: \n")
        allStaff[allStaff.size - 1].bonus += 200.0
        println(allStaff[allStaff.size - 1])
    }
}

class Rent(
    val colabName: String,
    val clientName: String,
    val mediaRent: MutableList<String>,
    val mediaType: MutableList<String>,
    val dateOfRent: String,
    var status: String,
    var colabReturn: String,
    val price: Double,
    var multa: Double
) {

    fun maxDayReturn(dateOfRent: String): String {
        val convertedDt = LocalDate.parse(dateOfRent, DateTimeFormatter.ofPattern("dd-MM-yyyy"))
        return convertedDt.plusDays(7).format(DateTimeFormatter.ofPattern("dd-MM-yyyy"))
    }

    override fun toString(): String {
        return "Funcionario(a) responsável: $colabName\nCliente: $clientName\nTítulo alugado: \n- ${
            mediaRent.joinToString(
                " \n- "
            )
        }\nTipo de mídia: \n- ${mediaType.joinToString(" \n- ")}\nData de saída: $dateOfRent\nData máxima de retorno: ${
            maxDayReturn(
                dateOfRent
            )
        }\nStatus: $status\nFuncionario(a) reponsável pela devolução: $colabReturn\nPreço: R$ " + "%.2f".format(
            price
        ) + "\nMulta Aplicada: R$ " + "%.2f".format(multa) + "\n"
    }
}


