# ack-pdfbox
Java code for specific pdf manipulations

### Table of Contents

- [Purpose](#purpose)
- [CLI Test Commands](#cli-test-commands)
    - [PDFBox Version](#pdfbox-version)
    - [Read Acroform Output Json File](#read-acroform-output-json-file)
    - [Fill Acroform From Json File](#fill-acroform-from-json-file)
- [Sample Code](sample-code)
    - [Sample Read Acroform Json File Result](#sample-read-acroform-json-file-result)
    - [Generate Certificates and KeyStore](#generate-certificates-and-keystore)
- [API](api)
    - [read](#read)
    - [fill](#fill)
    - [add-image](#add-image)
    - [Encrypt](#encrypt)
    - [Decrypt](#decrypt)

## Purpose
To have a uniform and standardized method for populating PDF Acroforms. A JSON file is produced when reading a PDF Acroform and that same JSON file can be used to turn around and populate the source PDF.

## CLI Test Commands
Open ack-pdfbox in a terminal command prompt and then run the following commands

### PDFBox Version
```
java -jar dist/ackpdfbox-1.0-SNAPSHOT-jar-with-dependencies.jar -version
```

### Read Acroform Output Json File
```
java -jar dist/ackpdfbox-1.0-SNAPSHOT-jar-with-dependencies.jar read test/i-9.pdf test/i-9.pdf.json
```

### Fill Acroform From Json File
```
java -jar dist/ackpdfbox-1.0-SNAPSHOT-jar-with-dependencies.jar fill test/i-9.pdf test/i-9-with-sig.pdf.json test/i-9-with-sig.pdf
```

### Encrypt PDF
```
java -jar dist/ackpdfbox-1.0-SNAPSHOT-jar-with-dependencies.jar Encrypt <pdf-path> <pdf-out-path> <options>
```

### Decrypt PDF
```
java -jar dist/ackpdfbox-1.0-SNAPSHOT-jar-with-dependencies.jar Encrypt <pdf-path> <pdf-out-path> <options>
```

## Sample Code

### Sample Read Acroform Json File Result
```
[{
  "fullyQualifiedName": "form1[0].#subform[6].FamilyName[0]",
  "isReadOnly": false,
  "partialName": "FamilyName[0]",
  "type": "org.apache.pdfbox.pdmodel.interactive.form.PDTextField",
  "isRequired": false,
  "page": 6,
  "cords": {
    "x": "39.484",
    "y": "597.929",
    "width": "174.00198",
    "height": "15.119995"
  },
  "value": "Apple"
}]
```

### Sample Json File Used For Acroform Fill

The JSON file below will fill two fields.

- First field is a plain text field
- Second field will be replaced by a base64 image of a hand-signature
    - **remove** was added to delete field from pdf
    - **base64Overlay** was added to insert hand-signature image where field was
        - **uri** specifies jpg or png image data
        - **forceWidthHeight** forces image to fit with-in field coordinates

```
[{
  "fullyQualifiedName": "form1[0].#subform[6].FamilyName[0]",
  "isReadOnly": false,
  "partialName": "FamilyName[0]",
  "type": "org.apache.pdfbox.pdmodel.interactive.form.PDTextField",
  "isRequired": false,
  "page": 6,
  "cords": {
    "x": "39.484",
    "y": "597.929",
    "width": "174.00198",
    "height": "15.119995"
  },
  "value": "Apple"
},{
  "fullyQualifiedName": "form1[0].#subform[6].EmployeeSignature[0]",
  "isReadOnly": true,
  "partialName": "EmployeeSignature[0]",
  "type": "org.apache.pdfbox.pdmodel.interactive.form.PDTextField",
  "isRequired": false,
  "page": 6,
  "cords": {
    "x": "126.964",
    "y": "227.523",
    "width": "283.394",
    "height": "15.12001"
  },
  "remove": true,
  "base64Overlay": {
    "uri": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAokAAACDCAYAAADh04FuAAAAAXNSR0IArs4c6QAAAVlpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IlhNUCBDb3JlIDUuNC4wIj4KICAgPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4KICAgICAgPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIKICAgICAgICAgICAgeG1sbnM6dGlmZj0iaHR0cDovL25zLmFkb2JlLmNvbS90aWZmLzEuMC8iPgogICAgICAgICA8dGlmZjpPcmllbnRhdGlvbj4xPC90aWZmOk9yaWVudGF0aW9uPgogICAgICA8L3JkZjpEZXNjcmlwdGlvbj4KICAgPC9yZGY6UkRGPgo8L3g6eG1wbWV0YT4KTMInWQAALL9JREFUeAHtnWmsXdV1xzMnzaA4aUmkJq1P2/RDhpKbfqj6IZZulA9VaKXctqqoRCpfVYSqRCqPBqmgFHxLpIKgjR+kwjSA/aKEBAGJX5hiCLEfCRTC5GczGTP4GTAQk8KDDGZ0+v+bu8t6y/tMdzzn3v+S1tvz3mv/zj7n7LPPvue97nUSERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABERABEcgh0ET6Vuivob+EXgZNoBIREAEREAEREAEREIEpJjCPvnOC6PUFxP0cejE0gUpEoB8CTRTeBN3TdRO4EhEQAREQAREQgQoT8JPDWHgX7E8q3AeZVn0Ct8BEO7bOqr7JslAEREAEREAEpptA2kqivaHTv2G6Man3fRJ4BeXtmHoe4aTPOlVcBERgAgm8fgL7pC6JQF0JJDB8J/RdOR14FukN6FJOPiWLQIwAJ4heTkTErI+sQHg1bEi6dnwC7iroR6EfhrIfF0LPgUpEQASGQECTxCFAVZUi0AeBI1H2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",
    "forceWidthHeight": true
  }
}]
```

### Generate Certificates and KeyStore
If you will be running encrypt/decrypt functionality WITH certificate based security, get ready to run some terminal commands against Java's keytool.

> In a terminal command prompt window, run the following in a folder where certificate files can live

Step #1 Create keyStore
```
keytool -genkey -keyalg RSA -alias pdfbox-test-alias -keystore pdfbox-test-keystore.jks -storepass pdfbox-test-password -validity 360 -keysize 2048
```

Step #2 Create a selfsigned certificate
```
keytool -export -alias pdfbox-test-alias -file pdfbox-test.crt -keystore pdfbox-test-keystore.jks
```

Step #3 Marry the certificate and keyStore together as a .p12 file
```
keytool -importkeystore -srckeystore pdfbox-test-keystore.jks -destkeystore pdfbox-test.p12 -srcstoretype JKS -deststoretype PKCS12 -deststorepass pdfbox-test-password -srcalias pdfbox-test-alias -destalias pdfbox-test-p12
```

You should now have the following files in targeted folder:

- pdfbox-test.crt
    - used to encrypt
- pdfbox-test-keystore.jks
    - used to create p12 file below
- pdfbox-test.p12
    - used to decrypt


### MAY Need Java Cryptography
Depending on your level of advanced encryption needs, you (may) need to install [Java Cryptography](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html)


## API

### read
Read Acroform fields from a PDF

- **pdfPath** - The PDF file to read
- **jsonPath** - optional, path to write JSON file otherwise output to console

### fill
Fill Acroform fields from a PDF

- **pdfPath** - The PDF file to fill
- **jsonPath** - The json file to fill pdf with
- **outPath** - The file to save the decrypted document to. If left blank then it will be the same as the input file || options

### add-image
- **pdfPath** - The PDF file to encrypt
- **outPath** - The file to save the decrypted document to. If left blank then it will be the same as the input file || options
- **page** - The page number where to drop image
- **x** - The x cord where to drop image
- **y** - The y cord where to drop image

### Encrypt
- **pdfPath** - The PDF file to encrypt
- **outPath** - The file to save the decrypted document to. If left blank then it will be the same as the input file || options
- **options** - {}
    - **O**:                          The owner password to the PDF, ignored if -certFile is specified.
    - **U**:                          The user password to the PDF, ignored if -certFile is specified.
    - **certFile**:                   Path to X.509 cert file.
    - **canAssemble**:                true  Set the assemble permission.
    - **canExtractContent**:          true  Set the extraction permission.
    - **canExtractForAccessibility**: true  Set the extraction permission.
    - **canFillInForm**:              true  Set the fill in form permission.
    - **canModify**:                  true  Set the modify permission.
    - **canModifyAnnotations**:       true  Set the modify annots permission.
    - **canPrint**:                   true  Set the print permission.
    - **canPrintDegraded**:           true  Set the print degraded permission.
    - **keyLength**:                  40, 128 or 256  The number of bits for the encryption key. For 128 and above bits Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files must be installed.

### Decrypt
- **pdfPath** - The PDF file to decrypt
- **outPath** - The file to save the decrypted document to. If left blank then it will be the same as the input file || options
- **options** - {}
    - **password**: Password to the PDF or certificate in keystore.
    - **keyStore**: Path to keystore that holds certificate to decrypt the document (typically a .p12 file). This is only required if the document is encrypted with a certificate, otherwise only the password is required.
    - **alias**:    The alias to the certificate in the keystore.
