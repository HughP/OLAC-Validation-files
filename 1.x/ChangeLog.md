# Change log

24 January 2023 The dcterms file was changed to come into conformance with the current DC documentation. What was done? – Allow RFC5646 as a qualifier on language and add NLM as a qualifier to subjects.

## Discussion


Following Hugh Paterson's presentation in Berlin on serials in OLAC and a recent expression of interest to send materials to OLAC via OAI; Hugh created the linked repo with inter-linked records for:

* The Linking record of a Journal
* The Journal
* The first Volume of a Journal
* The first and second Issue of a Journal
* The first two Articles of the first issue.

the file can be found here: https://github.com/HughP/OLAC-Type-Values/blob/main/Minimum-Viable-Journal-LinguisticDiscovery-OLAC.xml

Hugh tried to validate the repo via the online validator. Several problems remain. These are listed below:

1. Some logical equivelences are not validating. For example, `<dc:description xsi:type="dcterms:abstract">` seems to logically be the same as `<dcterms:abstract>` but the validator doesn't recognize the first case. No doubt it is due to the structure of the XSD file that OLAC references for dcterms. —— but why is the logical jump prevented? The workable solution is just to change code in crosswalks or static repos, some clarification would be well-recived if someone could explain why these things are not equivalent.
2. OLAC documentation supports the use of MARC Relator Roles but then the validator doesn't support those identities. This would be fixed if the XSD file at http://www.language-archives.org/OLAC/1.1/olac.xsd were to be modified to include the marcrel namespace via an `<xs:include>` statement and a pointer to a local file. The following is an outline of options. The recomendation is to implement marcrel as a valid xsi:type namespace on dc:contributor elements with type validation. This is described as list item "E" below: Someone with server access will need to do the work if any action is to take place.

A. We could do nothing. This should be rejected as an option because the documentation says that OLAC supports these roles.

B. We could change the documentation to remove reference to the MARC relator Roles. This should be rejected as these roles are helpful to archives and are useful in description.

C. We can add MARC roles to the OALC namespace. This should be rejected. If we took this route it would mean invoking fewer files and namespaces in the validation process but some OLAC roles have the same names or codes as MARC roles but maintain incompatible semantics. This leads to an analysis that both name spaces are needed.

D. We could modify the OLAC xsd validation files to include MARC relator roles as a possible attribute without any type validation on role terms used by using the linked xsd file http://www.ukoln.ac.uk/metadata/nisomi/cd-xml/2005-08-21/xmls/marcrel.xsd. This is as far as I know the only marcrel.xsd file mentioned in the literature and part of the major application profiles—this one I believe was part of the eprints project. My understanding by reading the code is that this file does not allow for content (MARC role) validation. It essentially allows for all three letter strings— no type checking. Therefore "option D" is NOT recommended. Across the internet there are projects which reference xsi:schemaLocation="http://www.loc.gov/marc.relators/ http://imlsdcc2.grainger.illinois.edu/registry/marcrel.xsd" the website on illinois.edu is long gone with no stash even in the internet archive. An alternative to the xmlns declaration for marcrel can be seen as <xs:import namespace="http://id.loc.gov/vocabulary/relators/" schemaLocation="marcrel.xsd"/> That is the URI has in some projects changed to http://id.loc.gov/vocabulary/relators/ which references the Library of Congress's current URI the MARC relators in their linked data portal. Each relator role now has a URI, whereas in 2005 when the eprints project did their work there were no specific relators only one url with hash tags. So while no prefered file for type checking exists it seems that some projects do as OLAC does and hosts local versions which are not fully accessible. Some other projects have only adopted specific roles into their application profile. For example this one: https://github.com/mbuechner/mint2/blob/c5225e7a8509302b61bfc985e2143fa24354f2f9/schemas/DDB-DC_in_RDF-XML_XSDv1/marcrel.xsd

E. The recommendation is to implement marcrel as a valid xsi:type namespace on dc:contributor elements with type validation. This fulfills the statement in the documentation and also continues to make the validator useful. If this path is chosen we need to know what sort of XSD file to create. For example, one step forward in this area is to repurose the file produced by the Repository of Ireland found here: https://github.com/Digital-Repository-of-Ireland/dri-app/blob/0ea39f7f31610346fc0d3b0e5d20e79c9a4fa4fa/config/schemas/marcrel.xsd Or to just simply copy out the codes and terms from the Library of Congress site to look more like the OLAC role extension found here: http://www.language-archives.org/OLAC/1.1/olac-role.xsd  For ease here is the MARC relator role site https://www.loc.gov/marc/relators/relaterm.html. There are ~270 valid codes, though some may be deprecated. The included CSV file does not indicated deprecated status for roles no-longer in use. The CSV file might be useful if one were to make an OLAC specific XSD file like the olac-role.xsd file. From a User Experience point of view, the recommendation is to put all of the codes in the validator and then for the deprecated codes provide an error message with the code of most current usage. This is more useful than a generic code saying: ""X" code is not valid". Though all codes not in the ~270 listed here should have a "not a valid code"  message... So three options: "valid", "not valid", "not valid due to depreciation please use "Y" code instead". I am not sure if the error codes are in some way included in the xsd file or if they are a custom setting in the xml validator.

Following the resolution of issue number two above (with opinionated preference for action course E) I will be willing to make adjustments to the journal demonstration static repo so that it validates. Then I will be willing to do an annotated version for instructional purposes. Together these resources can be used to create a best practices recommendation.

