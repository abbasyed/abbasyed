<h1>Hi, I'm Abbas Syed 👋<h1>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitHub Stats</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <canvas id="myChart" width="400" height="400"></canvas>

    <script>
      
        const accessToken = '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';

      
        const query = `
        {
          user(login: "AbbasHussainSyed") {
            name
            repositoriesContributedTo(first: 100, contributionTypes: [COMMIT, ISSUE, PULL_REQUEST], includeUserRepositories: true) {
              totalCount
            }
            repositories(first: 100, ownerAffiliations: OWNER, orderBy: {field: STARGAZERS, direction: DESC}) {
              totalCount
            }
            contributionsCollection(from: "2023-01-01T00:00:00Z") {
              totalCommitContributions
              totalPullRequestContributions
              totalIssueContributions
            }
          }
        }
        `;

    
        fetch('https://api.github.com/graphql', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${accessToken}`
            },
            body: JSON.stringify({ query: query }),
        })
        .then(response => response.json())
        .then(data => {
           
            const { name, repositoriesContributedTo, repositories, contributionsCollection } = data.data.user;
            const totalStars = repositories.totalCount;
            const totalCommits = contributionsCollection.totalCommitContributions;
            const totalPRs = contributionsCollection.totalPullRequestContributions;
            const totalIssues = contributionsCollection.totalIssueContributions;
            const contributedLastYear = repositoriesContributedTo.totalCount;

           
            const ctx = document.getElementById('myChart').getContext('2d');
            const myChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Total Stars Earned', 'Total Commits (2024)', 'Total PRs', 'Total Issues', 'Contributed to (last year)'],
                    datasets: [{
                        label: `${name}'s GitHub Stats`,
                        data: [totalStars, totalCommits, totalPRs, totalIssues, contributedLastYear],
                        backgroundColor: [
                            'rgba(255, 99, 132, 0.2)',
                            'rgba(54, 162, 235, 0.2)',
                            'rgba(255, 206, 86, 0.2)',
                            'rgba(75, 192, 192, 0.2)',
                            'rgba(153, 102, 255, 0.2)'
                        ],
                        borderColor: [
                            'rgba(255, 99, 132, 1)',
                            'rgba(54, 162, 235, 1)',
                            'rgba(255, 206, 86, 1)',
                            'rgba(75, 192, 192, 1)',
                            'rgba(153, 102, 255, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });
        })
        .catch(error => console.error('Error:', error));
    </script>
</body>
</html>
