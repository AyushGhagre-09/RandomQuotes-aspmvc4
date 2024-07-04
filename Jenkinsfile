node {
    def msBuildTool = tool 'msbuild-2019'
    println msBuildTool
    try {
        stage('Checkout SCM') {
            // Checkout the source code from the SCM repository
            checkout scm
        }

        stage('Restore NuGet Packages') {
            // Restore NuGet packages
           // bat 'C:\\nuget\\nuget.exe restore RandomQuotes.sln'
        }

        stage('Build Solution with MSBuild') {
            // Build the solution using MSBuild and run OctoPack
            bat '${msBuildTool}\\MSBuild.exe RandomQuotes.sln /p:RunOctoPack=true /p:OctoPackPackageVersion=1.0.${BUILD_NUMBER} /p:OctoPackEnforceAddingFiles=true'
        }

        stage('Run NUnit Tests') {
            // Run NUnit tests
            bat '.\\packages\\NUnit.ConsoleRunner.3.10.0\\tools\\nunit3-console.exe .\\RandomQuotes.Tests\\bin\\Debug\\RandomQuotes.Tests.dll --result=TestResult.xml'
        }

        stage('Publish Test Results') {
            // Publish test results to Jenkins
            junit 'TestResult.xml'
        }
    } catch (Exception e) {
        // Handle exceptions
        echo "Pipeline failed with error: ${e.message}"
        currentBuild.result = 'FAILURE'
        throw e
    }
}
