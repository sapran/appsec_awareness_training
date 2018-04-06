Review OWASP [Application Security Verification Standard](https://www.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project) as a methodology basis for security code review.

Install [SonarQube](https://www.sonarqube.org/#downloads) or register for SonarCloud.io.

Analyze DVWA with SonarCloud
- git clone https://github.com/ethicalhack3r/DVWA
- Install Sonar Scanner client https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner
- Create the organization and the project, generate token.
- Run the scanner and demonstrate the results.

Sonar Cloud:
~~~
sonar-scanner \
  -Dsonar.projectKey=dvwa-me \
  -Dsonar.organization=dvwa \
  -Dsonar.sources=. \
  -Dsonar.host.url=https://sonarcloud.io \
  -Dsonar.login=<API_KEY>
~~~

Standalone SonarQube:
~~~
sonar-scanner \
  -Dsonar.projectKey=dvwa \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=<API_KEY>
~~~

Practice: exercise software security review on artificial cases (see handouts). Perform exercises in teams of 4 to 6 people. Review the results afterwards with on-screen demonstration.