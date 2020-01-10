Generate a list of all installed plugins on Jenkins:

Jenkins.instance.pluginManager.plugins.each{
  plugin -> 
    println ("${plugin.getShortName()}:${plugin.getVersion()}")
}

Running groovy DSL from the Jenkins script console:

import javaposse.jobdsl.dsl.*
import  javaposse.jobdsl.plugin.*

JenkinsJobManagement jm = new JenkinsJobManagement(System.out, [:], new File('.'));
DslScriptLoader dslScriptLoader = new DslScriptLoader(jm)
dslScriptLoader.runScript("folder('project-a')")