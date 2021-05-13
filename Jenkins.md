# Jenkins

## Artefakty

### Archiwizuj

```groovy
archiveArtifacts([
    allowEmptyArchive: true,
    artifacts: 'output/**',
    caseSensitive: true,
    defaultExcludes: true,
    fingerprint: false,
    onlyIfSuccessful: false
])
```

### Kopiuj

```groovy
copyArtifacts([
    projectName: "Builds/Something/Name",
    filter: 'target/*-somethin.jar',
    selector: lastSuccessful(),
    flatten: true
])
```

## Credentiale

```groovy
withCredentials([
    usernamePassword([
        credentialsId: 'CREDENTIAL_ID_USERA',
        usernameVariable: 'ADMIN_USERNAME',
        passwordVariable: 'ADMIN_PASSWORD'
    ]),
    string([
        credentialsId: 'CREDENTIAL_ID_API_KEY',
        variable: 'ADMIN_API_KEY'
    ]),
    sshUserPrivateKey([
        credentialsId: 'CREDENTIAL_ID_KLUCZ_SSH',
        keyFileVariable: 'ADMIN_SSH_KEY',
        passphraseVariable: '',
        usernameVariable: 'admin'
    ])
]) {
    // ...
}
```

## Czekaj

```groovy
sleep([
    unit: 'SECONDS',
    time: sleepTime
])
```

## Czekaj aż coś zwróci true

```groovy
waitUntil {
    // ...

    if (res.results != null) {
        return true
    } else {
        sleep([
            unit: 'SECONDS',
            time: 10
        ])

        return false
    }
}
```

## Czyszczenie Workspacea

```groovy
cleanWs()
```

## Dynamiczne stage wykonywane równolegle

```groovy
def stages = [:] // tak się tworzy tablicę asocjacyjną
def items = ['a', 'b', 'c']

items.each {
    stages[it] = { ->
        stage("Stage ${it}") {
            // ...
        }
    }
}

parallel stages
```

## Docker

### Buduj

```groovy
docker.build(
    "nazwa:tag",
    "--build-arg ARGUMENT=wartosc ."
)
```

### Buduj i wypchnij

```groovy
def image = docker.build(
    "nazwa:tag",
    "--build-arg ARGUMENT=wartosc ."
)
image.push()
```

### Użyj registry

```groovy
docker.withRegistry('adres.registry.com', credentialeDoRegistry) {
    // ...
}
```

## Git checkout

### Prosty

```groovy
git([
    url: 'git@github.com:uzyszkodnik-lub-organizacja/i-repo.git',
    branch: 'master',
    credentialsId: TUTAJ_CREDENTIAL_ID,
    poll: true
])
```

### Zaawansowany

```groovy
checkout([
    scm: [
        $class: 'GitSCM',
        branches: [[ name: '*/master' ]],
        userRemoteConfigs: [
            [
                credentialsId: TUTAJ_CREDENTIAL_ID,
                url: 'git@github.com:uzyszkodnik-lub-organizacja/i-repo.git'
            ]
        ]
    ]
])
```

### Tylko jednego katalogu

```groovy
checkout([
    scm: [
        $class: 'GitSCM',
        branches: [[ name: '*/master' ]],
        extensions: [
            [
                $class: 'SparseCheckoutPaths',
                sparseCheckoutPaths: [
                    [
                        path: 'katalog'
                    ]
                ]
            ]
        ],
        userRemoteConfigs: [
            [
                credentialsId: TUTAJ_CREDENTIAL_IT,
                url: 'git@github.com:uzyszkodnik-lub-organizacja/i-repo.git'
            ]
        ]
    ]
])

sh('mv katalog/* .')
```

### Razem z subrepo

```groovy
checkout([
    $class: 'GitSCM',
    branches: [[name: env.BRANCH_NAME]],
    doGenerateSubmoduleConfigurations: false,
    extensions: [
        [
            $class: 'CloneOption',
            depth: 1,
            noTags: false,
            reference: '',
            shallow: true,
            timeout: 10
        ],
        [
            $class: 'SubmoduleOption',
            disableSubmodules: false,
            parentCredentials: true,
            recursiveSubmodules: true,
            reference: '',
            trackingSubmodules: false
        ]
    ],
    submoduleCfg: [],
    userRemoteConfigs: [
        [
            credentialsId: TUTAJ_CREDENTIAL_IT,
            url: 'git@github.com:uzyszkodnik-lub-organizacja/i-repo.git'
        ]
    ]
])
```

## Iteruj po tablicy

```groovy
['a', 'b', 'c'].each {
    print(it)
}
```

## Klauzula try-catch-finally

```groovy
try {
    ...
} catch (err) {
    // err.message
} finally {

    stage('Clean up') {
        cleanWs()
    }

}
```

## Koloruj output

```groovy
ansiColor('xterm') {
    ...
}
```

## Operacje na plikach

### Czytaj plik tekstowy

```groovy
String txt = readFile('./sciezka/do/pliku')
```

### Zapisz plik tekstowy

```groovy
writeFile([
    file: 'plik.txt', 
    text: "Hello World\nNew Line"
])
```

## Pobierz zawartość adresu URL (za pomocą GET)

```groovy
String resp = new URL("http://example.org").getText()
```

## Pythonowy Virtualenv

```groovy
withPythonEnv('/usr/bin/python3') {
    ...
}
```

## Regexp

```groovy
def matches = (subject =~ /^(grupa)$/)

if (matches) {
    String grupa = matches[0][1]

    // Clean matches - important! Otherwise Jenkins will want to serialize it!
    matches = null
}
```

## Timeout

```groovy
timeout([
    time: 5,
    activity: true,
    unit: 'MINUTES'
]) {
    sh('npm run test')
}
```

## Triggeruj inny build

```groovy
build([
    job: "/Path/To/Job/Without/'jobs'",
    parameters: [
        string(name: 'STRING_PARAM', value: 'dupa'),
        booleanParam(name: 'SOME_FLAG', value: true)
    ],
    propagate: false,
    wait: false
])
```

## Status

### Aborted

```groovy
currentBuild.result = 'ABORTED'
error("Wolololo")
```

### Unstable

```groovy
unstable('Found Issues')
```

## Wykonaj coś w konkretnym katalogu

```groovy
String dirName = 'katalog'
sh("mkdir ${dirName}") // pamietaj żeby stworzyć katalog najpierw
dir(dirName) {
    // coś
}
```

## Wykonaj coś ze zmienną środowiskową

```groovy
withEnv(['ZMIENNA=wartosc']) {
    // coś
    print(env.ZMIENNA)
}
```

## Wykonaj komendę i przypisz output do zmiennej

```groovy
something = sh(
    script: "uname -a",
    returnStdout: true
)
```

Note: Użyj `.trim()` na końcu, aby pozbyć się białych znaków.
