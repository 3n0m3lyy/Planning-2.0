<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planning - Contrôle de Gestion</title>
    <style>
    body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 0;
    }

    h1 {
        font-weight: bold;
        margin-top: 20px;
        text-align: center;
        border: 2px solid black; /* Ajout d'une bordure */
        padding: 10px; /* Ajout de rembourrage */
        border-radius: 10px;
        background-color: #7FE8AB;/* Ajout de bordure arrondie */
    }

    #month-selection,
    #calendar {
        margin-bottom: 20px; /* Ajout de marge inférieure */
    }

    .content-container {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        max-width: 800px;
        margin: 20px;
    }

    .calendar {
        display: grid;
        grid-template-columns: repeat(7, 1fr);
        border-collapse: collapse;
        margin-bottom: 20px;
        font-size: 20px;
        width: 100%;
    }

    .calendar-header {
        text-align: center;
        font-weight: bold;
        font-size: 24px;
    }

    .day {
    padding: 50px;
    border: 1px solid #ccc;
    cursor: pointer;
    width: 20%; /* Répartir les jours sur 10% de la largeur */
    text-align: center; /* Centrer le contenu des cellules */
}

    .selected {
        background-color: #6BE8E8;
    }

    #name-selection,
    #month-selection {
        text-align: left;
        margin-left: 10px;
    }

    #name-selection select,
    #month-selection select {
        width: 150px;
    }

    #name-selection label,
    #month-selection label {
        margin-right: 10px;
    }
</style>
</head>

<body>
    <h1>Planning - Contrôle de Gestion</h1>

    <div class="content-container">
        <div id="month-selection">
    <p>Veuillez sélectionner le mois et l'année puis actualiser le calendrier</p>
    <label for="month">Mois:</label>
    <select id="month">
        <option value="0">Janvier</option>
        <option value="1">Février</option>
        <option value="2">Mars</option>
        <option value="3">Avril</option>
        <option value="4">Mai</option>
        <option value="5">Juin</option>
        <option value="6">Juillet</option>
        <option value="7">Août</option>
        <option value="8">Septembre</option>
        <option value="9">Octobre</option>
        <option value="10">Novembre</option>
        <option value="11">Décembre</option>
    </select>
    <label for="year">Année:</label>
    <input type="number" id="year" min="1900" max="2100" value="2024">
    <button onclick="generateCalendar()">Actualiser le calendrier</button>
</div>

        <div id="calendar"></div>

        <div id="name-selection">
            <label for="name">Nom et Prénom:</label>
            <select id="name">
                <option value="Xavier GIOVANNINI">Xavier GIOVANNINI</option>
                <option value="Remi STAUDER">Remi STAUDER</option>
                <option value="Sandy COQUELET">Sandy COQUELET</option>
                <option value="Marie SURREL">Marie SURREL</option>
            </select>
        </div>

        <button onclick="validerConges()">Valider</button>
        <p id="resume-conges"></p>
    </div>

    <script>
    
        let selectedDates = [];

        function generateCalendar() {
            const nameInput = document.getElementById('name');
            const monthInput = document.getElementById('month');
            const yearInput = document.getElementById('year');
            const name = nameInput.value;
            const month = parseInt(monthInput.value);
            const year = parseInt(yearInput.value);

            const calendarDiv = document.getElementById('calendar');
            const daysOfWeek = ['Dimanche', '  Lundi  ', '  Mardi  ', 'Mercredi ', ' Jeudi  ', 'Vendredi', '  Samedi'];
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            const firstDayOfWeek = new Date(year, month, 1).getDay();

            let html = '<table class="calendar"><tr class="calendar-header">';
            daysOfWeek.forEach(day => {
                html += `<th>${day}</th>`;
            });
            html += '</tr><tr>';

            let dayCount = 1;
            for (let i = 0; i < 42; i++) {
                if (i < firstDayOfWeek || dayCount > daysInMonth) {
                    html += '<td></td>';
                } else {
                    const isSelected = selectedDates.includes(dayCount);
                    const className = isSelected ? 'selected' : 'day';
                    html += `<td class="${className}" onclick="toggleDate(${dayCount})">${dayCount}</td>`;
                    dayCount++;
                }
                if ((i + 1) % 7 === 0) {
                    html += '</tr><tr>';
                }
            }

            html += '</tr></table>';
            calendarDiv.innerHTML = html;
        }

        function toggleDate(date) {
            const index = selectedDates.indexOf(date);
            if (index === -1) {
                selectedDates.push(date);
            } else {
                selectedDates.splice(index, 1);
            }
            generateCalendar();
        }

        function validerConges() {
    const nameInput = document.getElementById('name');
    const name = nameInput.value;
    const monthInput = document.getElementById('month');
    const month = parseInt(monthInput.value);
    const yearInput = document.getElementById('year');
    const year = parseInt(yearInput.value);

    const selectedDatesString = selectedDates.length > 0 ? selectedDates.join(', ') : 'aucun jour sélectionné';
    const resumeConges = `Les jours de congés sélectionnés par ${name} pour ${getMonthName(month)} ${year} sont : ${selectedDatesString}`;
    document.getElementById('resume-conges').textContent = resumeConges;
}

function getMonthName(month) {
    const months = ['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin', 'Juillet', 'Août', 'Septembre', 'Octobre', 'Novembre', 'Décembre'];
    return months[month];
}

        generateCalendar();
    </script>

</body>

</html>

