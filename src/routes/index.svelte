<script>
import { onMount} from 'svelte';
import cytoscape from 'cytoscape';
import dagre from 'cytoscape-dagre';

cytoscape.use(dagre)

let station = 1
let selected_data = [];
let show_graph = true;
let show_criteria_list = false;
let stations = [];
let events = []
let data = {
    esg: 89.1,
    criteria: [
        {
            name: 'E',
            weight: 5,
            subcriterias: [
                {
                    name: 'Рациональное использование водных ресурсов',
                    weight: 10,
                    subcriterias: [
                        {
                            name: 'Объём технической (охлаждающей) воды',
                            score: 2,
                            weight: 2
                        },
                        {
                            name: 'Объём забираемой сетевой воды',
                            score: 4,
                            weight: 2
                        },
                        {
                            name: 'Объём сбрасываемых сточных вод',
                            score: 4,
                            weight: 2
                        },
                        {
                            name: 'Качество сбрасываемых сточных вод',
                            score: 3,
                            weight: 2
                        },
                        {
                            name: 'Объём водопотребления',
                            score: 4,
                            weight: 2
                        },
                        {
                            name: 'Качество воды',
                            score: 2,
                            weight: 2
                        },
                    ]
                },
                {
                    name: 'Токсичные выбросы и отходы',
                    weight: 4,
                    subcriterias: [
                        {
                            name: 'Объём выбрасываемых загрязняющих веществ',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Объём образования отходов',
                            score: 3,
                            weight: 3,
                        },
                        {
                            name: 'Накопление и размещение отходов',
                            score: 3,
                            weight: 3,
                        },
                        {
                            name: 'Взаимоотношения с подрядчиками',
                            score: 3,
                            weight: 3,
                        },
                        {
                            name: 'Критериальные значения контролируемых диагностических показателей (критерии безопасности)',
                            score: 3,
                            weight: 3,
                        },
                    ]
                },
                {
                    name: 'Выбросы углерода',
                    weight: 4,
                    subcriterias: [
                        {
                            name: 'Объём выбрасываемых загрязняющих веществ',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Углеродный след',
                            score: 2,
                            weight: 4
                        },
                    ]
                },
                {
                    name: 'Биоразнообразие и землепользование',
                    weight: 4,
                    subcriterias: [
                        {
                            name: 'Оценка влияния на экосистему отработанной воды в соответствии с изменением её химического состава после использования на предприятиии',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Соблюдение экологических требований и ведение экологического менеджмента',
                            score: 2,
                            weight: 4
                        },
                    ]
                },
                {
                    name: 'Качество воздуха и физическое воздействие',
                    weight: 4,
                    subcriterias: [
                        {
                            name: 'Средняя концентрация примеси в воздухе',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Максимальная разовая концентрация примеси',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Приведение и поддержание технического состояния машин и механизмов, в соответствии с нормативными требованиями по выбросам вредных веществ',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Уровень шума',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Уровень электромагнитного излучения',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Уровень вибрации',
                            score: 2,
                            weight: 4
                        },
                    ]
                },
                {
                    name: 'Энергоэффективность',
                    weight: 4,
                    subcriterias: [
                        {
                            name: 'Количественная оценка энергоэффективности мероприятий',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Коэффициент энергоёмкости полезного эффекта. КИУМ',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Эффективность генерации электро- / теплоэнергии. УРУТ',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Эффективность потребления энергоресурсов',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Запас срока службы турбин',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Запас срока службы котлов',
                            score: 2,
                            weight: 4
                        },
                    ]
                },
                {
                    name: 'Возможности в области чистых технологий (экологическая политика)',
                    weight: 4,
                    subcriterias: [
                        {
                            name: 'Проведение исследований и разработок по созданию нового оборудования, технологий и материалов (НИР и НИОКР)',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Выполнение работ по внедрению нового оборудования, технологий и материалов (модернизация)',
                            score: 2,
                            weight: 4
                        },
                    ]
                },
                {
                    name: 'Мероприятия по снижению выбросов',
                    weight: 4,
                    subcriterias: [
                        {
                            name: 'Проверка или аудит выбросов CO2',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Установление целей по сокращению выбросов CO2, указание крайних сроков',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Тенденция уровня CO2 за последние 3 года',
                            score: 2,
                            weight: 4
                        },
                        {
                            name: 'Воздействие изменения климата на изменения в компании, меры адаптации к климатическим изменениям',
                            score: 2,
                            weight: 4
                        },
                    ]
                },
            ]
        },
        {
            name: 'S',
            weight: 3,
            subcriterias: [
                {
                    name: 'Охрана труда и здоровье персонала',
                    weight: 1,
                    subcriterias: [
                        {
                            name: 'Системы в области промышленной безопасности, надёжности и целостности',
                            score: 3,
                            weight: 2
                        },
                        {
                            name: 'Работоспособность средств и систем защиты',
                            score: 2,
                            weight: 1
                        },
                        {
                            name: 'Коренные причины отказов оборудования и возникновения аварий',
                            score: 2,
                            weight: 1
                        }
                    ]
                },
                {
                    name: 'Развитие человеческого капитала',
                    weight: 1,
                    subcriterias: [
                        {
                            name: 'Процесс обучения основным навыкам профессии',
                            score: 3,
                            weight: 2
                        },
                        {
                            name: 'Оценка уровня квалификации и потенциала сотрудника ',
                            score: 4,
                            weight: 5
                        }
                    ]
                },
                {
                    name: 'Отношения с общественностью',
                    weight: 1,
                    subcriterias: [
                        {
                            name: 'Возникновение опасных природных явлений',
                            score: 3,
                            weight: 2
                        },
                        {
                            name: 'Ограничение доступа к транспортным маршрутам и объектам социальной инфраструктуры',
                            score: 4,
                            weight: 5
                        },
                        {
                            name: 'Землепользование и традиционная хозяйственная деятельность',
                            score: 4,
                            weight: 5
                        }
                    ]
                },
                {
                    name: 'Ответственное инвестирование и работа с клиентами',
                    weight: 1,
                    subcriterias: [
                        {
                            name: 'Уровень местных цен на важнейшие товары и услуги',
                            score: 3,
                            weight: 2
                        },
                        {
                            name: 'Финансовые показатели',
                            score: 4,
                            weight: 5
                        },
                        {
                            name: 'Расчет эффективности инвестиционных вложений с помощью различных показателей',
                            score: 4,
                            weight: 5
                        }
                    ]
                },
                {
                    name: 'Финансовая устойчивость',
                    weight: 1,
                    subcriterias: [
                        {
                            name: 'Сравнение каждой позиции отчётности с предыдущим периодом',
                            score: 3,
                            weight: 2
                        },
                        {
                            name: 'Запланируемые источники финансирования',
                            score: 4,
                            weight: 5
                        },
                        {
                            name: 'Анализ возможностей предприятия с точки зрения финансов',
                            score: 4,
                            weight: 5
                        },
                        {
                            name: 'Объективный анализ величины и структуры активов и пассивов предприятия',
                            score: 4,
                            weight: 5
                        },
                        {
                            name: 'Анализ соответствия финансово-хозяйственной деятельности предприятия целям его уставной деятельности',
                            score: 4,
                            weight: 5
                        }
                    ]
                },
            ]
        },
        {
            name: 'G',
            weight: 2,
            subcriterias: [
                {
                    name: 'Корпоративное управление',
                    weight: 2,
                    subcriterias: [
                        {
                            name: 'Случаи коррупции',
                            score: 2,
                            weight: 1
                        },
                        {
                            name: 'Отсутствие нарушений антимонопольного законодательства',
                            score: 3,
                            weight: 2
                        },
                        {
                            name: 'Наличие антимонопольной политики',
                            score: 3,
                            weight: 2
                        },
                        {
                            name: 'Диагностические показатели и признаки технического состояния объекта, частота возникновения аварийных ситуаций',
                            score: 3,
                            weight: 2
                        },
                    ]
                },
                {
                    name: 'Классификация персонала на предприятии',
                    weight: 4,
                    subcriterias: [
                        {
                            name: 'Персонал предприятия по имущественным отношениям',
                            score: 0,
                            weight: 2
                        },
                        {
                            name: 'Персонал предприятия по категориям',
                            score: 1,
                            weight: 3
                        }
                    ]
                },
                {
                    name: 'Эффективность Совета директоров',
                    weight: 3,
                    subcriterias: [
                        {
                            name: 'Наличие годового отчёта в открытом доступе',
                            score: 3,
                            weight: 2
                        },
                        {
                            name: 'Наличие принятых официальных документов Советом директоров',
                            score: 1,
                            weight: 3
                        },
                        {
                            name: 'Структура управления в компании',
                            score: 1,
                            weight: 3
                        },
                    ]
                }
            ]
        }
    ]
}
let update_station = async (subcriteria_scores) => {
    console.log(subcriteria_scores)
    data.criteria.forEach((crit, i) => {
        let counter = 0
        crit.subcriterias.forEach((subcrit, j) => {
            subcrit.subcriterias.forEach((_, k) => {
                data.criteria[i].subcriterias[j].subcriterias[k].score = subcriteria_scores[i][counter]
                data.criteria[i].subcriterias[j].subcriterias[k].weight = 1
                counter += 1
            })
            data.criteria[i].subcriterias[j].weight = 1
        })
        data.criteria[i].weight = 1
    })
    let sumcrit = 0
    let weightscrit = 0
    data.criteria.forEach((crit) => {
        let sumsubs = 0
        let weightssubs = 0
        crit.subcriterias.forEach((subcrit) => {
            let sumsubsubs = 0
            let weightssubsubs = 0
            subcrit.subcriterias.forEach((subsubcrit) => {
                sumsubsubs += subsubcrit.weight * subsubcrit.score
                weightssubsubs += subsubcrit.weight
            })
            subcrit.score = sumsubsubs / weightssubsubs
            sumsubs += subcrit.weight * (sumsubsubs / weightssubsubs)
            weightssubs += subcrit.weight
        })
        crit.score = sumsubs / weightssubs
        sumcrit += crit.weight * (sumsubs / weightssubs)
        weightscrit += crit.weight
    })
    data.esg = ((sumcrit / weightscrit / 4) * 100).toFixed(2)
    let elements = [{data: {id: 'esg', name: 'ESG'}, grabbable: false, classes: ['esg']}]
    data.criteria.forEach((el, i) => {
        elements.push({data: {id: `${i}`, name: el.name}, grabbable: false, classes: ['el']})
        elements.push({data: {id: `esg__${i}`, source: 'esg', target: `${i}`, weight: el.weight}, selectable: false})

        let sum = 0
        el.subcriterias.forEach((a) => sum += a.score)
        let avg = sum / el.subcriterias.length; 
        el.subcriterias.forEach((subel, j) => {
            elements.push({data: {id: `${i}_${j}`, name: subel.name}, grabbable: false, classes: ['subel', `${i}_subel`,`${subel.score < avg ? 'bad' : 'good'}`]})
            elements.push({data: {id: `${i}__${j}`, source: `${i}`, target: `${i}_${j}`, weight: subel.weight}, selectable: false})

            let sumsub = 0
            subel.subcriterias.forEach((a) => sumsub += a.score)
            let avgsub = sumsub / subel.subcriterias.length; 

            subel.subcriterias.forEach((subsubel, k) => {
                elements.push({data: {id: `${i}_${j}_${k}`, name: subsubel.name, score: subsubel.score}, grabbable: false, classes: ['subsubel', `${i}_${j}_subsubel`,`${subsubel.score < avgsub ? 'bad' : 'good'}`, `score_${subsubel.score}`]})
                elements.push({data: {id: `${i}__${j}__${k}`, source: `${i}_${j}`, target: `${i}_${j}_${k}`, weight: subsubel.weight}, selectable: false})
            })
        })
    })
    var cy = cytoscape({
        container: graph,
        elements: elements,
        zoom: 1,
        selectionType: 'additive',
        style: [
            {
                selector: '.subel',
                style: {
                    "text-margin-x": 5
                }
            },
            {
                selector: '.subsubel',
                style: {
                    "text-margin-x": 5
                }
            },
            {
                selector: '.named',
                style: {
                    'label': 'data(name)',
                    "text-valign": "top",
                    "text-halign": "right",
                    "text-margin-x": 15,
                    "text-wrap": "wrap",
                    "text-max-width": 300,
                    "text-background-color": "#fff",
                    "text-background-opacity": 1,
                    "text-background-shape": "round-rectangle",
                    "text-background-padding": 10,
                    "text-border-opacity": 1,
                    "text-border-width": 2,
                    "text-border-style": "solid",
                    "text-border-color": "#c0c0c0"
                }
            },
            {
                selector: 'edge',
                style: {
                    'label': 'data(weight)',
                    "curve-style": "straight",
                    // 'text-rotation': 'autorotate',
                    "line-opacity": 0.2,
                    "text-valign": "top",
                    "text-halign": "center",
                    // "text-margin-y": -10,
                }
            },
            {
                selector: '.el',
                style: {
                    'label': 'data(name)',
                    "text-valign": "center",
                    "text-halign": "center",
                    'background-color': '#60a3bc',
                    'shape': 'rectangle',
                    'color': 'white',
                    'height': 50,
                    'width': 50,
                }
            },
            {
                selector: '.subel',
                style: {
                    'background-color': '#82ccdd'
                }
            },
            {
                selector: '.good',
                style: {
                    'background-color': '#78e08f',
                    'color': 'green'
                }
            },
            {
                selector: '.bad',
                style: {
                    'background-color': '#e55039',
                    'color': 'red'
                }
            },
            {
                selector: '.esg',
                style: {
                    'label': 'data(name)',
                    "text-valign": "center",
                    "text-halign": "left",
                    "z-index": 999
                }
            },
            {
                selector: '.esg',
                style: {
                    'shape': 'hexagon',
                    'height': 70,
                    'width': 70,
                    "text-valign": "center",
                    "text-halign": "center",
                    "color": 'white',
                    'background-color': '#4a69bd'
                }
            },
            {
                selector: '.score_4',
                style: {
                    'shape': 'polygon',
                    'shape-polygon-points': [0, 1, -1, 0, 0, -1, 1, 0]
                }
            },
            {
                selector: '.score_3',
                style: {
                    'shape': 'polygon',
                    'shape-polygon-points': [0, 1, -1, 0, 0, -1, 1, 0, 0, 0]
                }
            },
            {
                selector: '.score_2',
                style: {
                    'shape': 'polygon',
                    'shape-polygon-points': [0, 1, -1, 0, 0, -1]
                }
            },
            {
                selector: '.score_1',
                style: {
                    'shape': 'polygon',
                    'shape-polygon-points': [0, 1, -1, 0, 0, 0]
                }
            },
            {
                selector: '.score_0',
                style: {
                    'shape': 'ellipse',
                    'height': 10,
                    'width': 10,
                }
            },
            {
                selector: '.subsubel:selected',
                style: {
                    'background-color': '#0c2461'
                }
            }
        ]
    })
    cy.on('select', (event) => {
        if (event.target.hasClass('esg') || event.target.hasClass('el') || event.target.isEdge()) return
        if (event.target.hasClass('subsubel')) {
            selected_data.push(event.target.data());
            console.log(event.target.data())
            selected_data = selected_data
        } else {
            event.target.addClass("named")
        }
    })
    cy.on('unselect', (event) => {
        selected_data = selected_data.filter(data => data.name !== event.target.data().name)
        event.target.removeClass("named")
    })
    // cy.elements('.esg').layout({name: 'circle', avoidOverlap: false}).run()
    // cy.elements('.el').layout({name: 'circle', radius: 0, startAngle: 1/5*Math.PI, clockwise: true, avoidOverlap: false}).run()
    // cy.elements(`.subel`).layout({name: 'circle', radius: 0, startAngle: -1/3*Math.PI, avoidOverlap: false}).run()
    // cy.elements(`.subsubel`).layout({name: 'circle', radius: 0, fit: true, avoidOverlap: false}).run()
    // setTimeout(() => {
    //     cy.elements('.esg').layout({name: 'circle'}).run()
    //     cy.elements('.el').layout({name: 'circle', radius: 150, startAngle: 1/5*Math.PI, clockwise: true, animate: false, animationDuration: 500}).run()
    //     cy.elements(`.subel`).layout({name: 'circle', radius: 150, startAngle: -1/3*Math.PI, animate: false, animationDuration: 500}).run()
    //     cy.elements(`.subsubel`).layout({name: 'circle', radius: 150, animate: false, animationDuration: 500}).run()
        
    //     setTimeout(() => {
    //         cy.elements(`.subel`).layout({name: 'circle', radius: 350, startAngle: -1/3*Math.PI, animate: false, animationDuration: 500}).run()
    //         cy.elements(`.subsubel`).layout({name: 'circle', radius: 350, animate: false, animationDuration: 500}).run()
    //         setTimeout(() => {
    //             cy.elements(`.subsubel`).layout({name: 'circle', radius: 700, animate: false, animationDuration: 500}).run()
    //         }, 500)
    //     }, 500)
    // }, 500)
    cy.elements('.esg').layout({name: 'circle'}).run()
    cy.elements('.el').layout({name: 'circle', radius: 150, startAngle: 1/5*Math.PI, clockwise: true, animate: false, animationDuration: 500}).run()
    cy.elements(`.subel`).layout({name: 'circle', radius: 350, startAngle: -1/3*Math.PI, animate: false, animationDuration: 500}).run()
    cy.elements(`.subsubel`).layout({name: 'circle', radius: 700, animate: false, animationDuration: 500}).run()
}

let graph;
onMount(async () => {
    let stations_resp = await fetch('http://127.0.0.1:5173/api/stations')
    let stations_resp_data = await stations_resp.json()
    stations = stations_resp_data.stations

    let events_resp = await fetch('http://127.0.0.1:5173/api/events')
    let events_resp_data = await events_resp.json()
    events = events_resp_data.events
})
$: fetch(`http://127.0.0.1:5173/api/station/${station}`).then(resp => resp.json()).then(data => update_station(data.scores))


</script>

<div class="custom_grid h-screen">
    <div class="w-screen bg-gray-800 text-white shadow-md p-3 grid grid-cols-2 items-center">
        <h1 class="text-3xl">ESG-каталог</h1>
        <div class="grid grid-cols-3 items-center">
            <div class="flex flex-row col-span-2 gap-2">
                <p class="text-lg"><strong>Рейтинг:</strong> {data.esg}</p>
                <p class="text-lg"><strong>E:</strong> {(data.criteria[0].score * 25).toFixed(2)}</p>
                <p class="text-lg"><strong>S:</strong> {(data.criteria[1].score * 25).toFixed(2)}</p>
                <p class="text-lg"><strong>G:</strong> {(data.criteria[2].score * 25).toFixed(2)}</p>
            </div>
            <select class="bg-gray-700 appearance-none p-1" bind:value={station}>
                {#each stations as station, i}
                    {#if i === 0}
                    <option value={station.id} selected>{station.name}</option>
                    {:else}
                    <option value={station.id}>{station.name}</option>
                    {/if}
                {/each}
            </select>
        </div>
    </div>
    <div class="grid grid-cols-4 duration-300">
        <div class={`col-span-${show_criteria_list ? '2' : '1'} bg-gray-600 text-white shadow-xl p-3 grid grid-rows-6`}>
            <div class="flex items-start flex-col justify-center row-span-5 overflow-scroll"> 
                {#if show_criteria_list}
                <h1 class="text-2xl text-center">Список критериев</h1>
                {#each data.criteria as criteria}
                <p class="text-center text-xl">{criteria.name}-критерий</p>
                <div class="overflow-y-scroll h-40 p-3">
                {#each criteria.subcriterias as subcriteria}
                    {#each subcriteria.subcriterias as subsubcriteria}
                        <div class="flex justify-between items-center p-2 bg-gray-500 mb-2 rounded-xl">
                            <div>
                                <p class="text-lg">{subsubcriteria.name}</p>
                                <p class="text-sm">{subcriteria.name}</p>
                            </div>
                            <div>
                                <p class="text-3xl">{subsubcriteria.score}/4</p>
                            </div>
                        </div>
                    {/each}
                {/each}
                </div>
                {/each}
                {:else if selected_data.length === 0}
                <h1 class="text-lg text-center">Нажмите на критерий чтобы получить информацию</h1>
                {:else}
                {#each selected_data as data}
                <h1 class="text-lg text-left">- {data.name}, <strong>оценка: {data.score}</strong></h1>
                {/each}
                {/if}
            </div>
            <div class="row-span-1 flex flex-row items-center justify-center gap-2">
                <button class="p-2 bg-emerald-600 rounded-lg hover:shadow-lg hover:scale-105 duration-300" on:click={() => show_graph = !show_graph}>
                    {#if show_graph}
                    Список мероприятий
                    {:else}
                    Граф критериев
                    {/if}
                </button>
                <button class="p-2 bg-emerald-600 rounded-lg hover:shadow-lg hover:scale-105 duration-300" on:click={() => show_criteria_list = !show_criteria_list}>
                    {#if show_criteria_list}
                    Закрыть список
                    {:else}
                    Список критериев
                    {/if}
                </button>

            </div>
        </div>
        <div class={`${show_criteria_list ? 'col-span-2' : 'col-span-3'} ${show_graph ? '' : 'hidden'}`} bind:this={graph}></div>
        <div class={`${show_criteria_list ? 'col-span-2' : 'col-span-3'} ${show_graph ? 'hidden' : ''} p-3`}>
            <h1 class="text-center text-3xl mb-3">Мероприятия</h1>
            <div class="grid grid-cols-2 gap-2">
                {#each events as event}
                <div class="col-span-1 bg-gray-500 text-white p-2 rounded-xl">
                    <p class="text-lg">{event.name}</p>
                    <p class="text-sm">{event.description}</p>
                </div>
                {/each}
            </div>
        </div>

    </div>
</div>

<style>
.custom_grid {
    display: grid;
    grid-template-rows: min-content 1fr;
}
</style>
