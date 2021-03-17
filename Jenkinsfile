
node {
    stage('approve') {
        //def reportContents = readFile("${WORKSPACE}/build.xml")
        def reportContents = '''
                                <testsuites>
                                        <testsuite>
                                            <testcase available="14" name="2">
                                                <failure text="some_failure" id="1">
                                                    abc
                                                </failure>              
                                            </testcase>
                                        </testsuite>       
                                </testsuites> 
                            '''
        def statuses  = xmlReadStats(reportContents)     
    }
}

@NonCPS
def xmlReadStats(reportContents) {
    def testStatus = [:] 
    def parentNode = new XmlParser().parseText(reportContents) 
    def nodeList = parentNode.depthFirst()    
    def failuresPerTest = [:]

    nodeList.findAll { node -> node.name() == 'failure' }
    .each {
        def testNode = it.parent()
        def testName = testNode.attributes()['name']
        failuresPerTest[testName] = failuresPerTest[testName] != null ? failuresPerTest[testName]++ : 0
    } 

    if (parentNode != null && parentNode.attributes()!=null){         
        testStatus.total = nodeList.findAll { node -> node.name() == 'testcase' }.size()
        testStatus.failed = failuresPerTest.size()
        testStatus.skipped = 0
        testStatus.passed = testStatus.total - testStatus.failed - testStatus.skipped         
    }
    println(testStatus)
    return testStatus
}







