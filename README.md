# Creating a new project using the official Maven archetype

Following [Cucumber official tutorial](https://cucumber.io/docs/guides/10-minute-tutorial/).

      mvn archetype:generate "-DarchetypeGroupId=io.cucumber" "-DarchetypeArtifactId=cucumber-archetype" "-DarchetypeVersion=6.7.0" "-DgroupId=fund.theherd" "-DartifactId=cucumberseed" "-Dpackage=cucumberseed" "-Dversion=0.0.1" "-DinteractiveMode=false"

# Generating Gradle project

      cd cucumberseed
      gradle init

Answer "yes" when prompted:

      Found a Maven build. Generate a Gradle build from this? (default: yes) [yes, no] yes

Add dependency configuration to build.gradle

      configurations {
         cucumberRuntime {
            extendsFrom testImplementation
         }
      }

Add task to build.gradle

      task cucumber() {
         dependsOn assemble, compileTestJava
         doLast {
            javaexec {
               main = "io.cucumber.core.cli.Main"
               classpath = configurations.cucumberRuntime + sourceSets.main.output + sourceSets.test.output
               args = ['--plugin', 'pretty', '--glue', 'hellocucumber', 'src/test/resources']
            }
         }
      }

