The files in this directory are "meta-templates" to be used to create 
Simulator configuration (device family and version)-specific Subliminal trace 
templates, as required by the `subliminal-test` script to run tests using the 
Simulator.

When certain variables in one of these templates are replaced with suitable values, 
and the template converted into binary, it will then be suitable for profiling 
an application and outputting the results into the desired directory using the 
Instruments GUI, without needing to GUI-script the configuration of Instruments. 
`run_template.scpt` (in `Supporting Files/CI`) may be used to run the template.

It is necessary to use the Instruments GUI, rather than the `instruments` command-line 
tool, because that tool will not run tests using any simulator SDK but the latest:
http://stackoverflow.com/questions/12836129/launch-a-specific-hardware-version-of-ios-simulator-using-instruments-command-li

### The Meta-template Format

Each meta-template was created by:

1.  opening a substituted `Subliminal.tracetemplate` (i.e. that installed in 
    `~/Library/Application Support/Instruments/Templates`) in Instruments
2.  setting a target (i.e. `Subliminal Integration Tests`) and configuration
3.  checking "Continuously Log Results" and setting a valid results output directory
4.  saving the result as a new template
5.  converting the plist to XML using `plutil`
6.  replacing the following values with substitution variables:
    * The user's home directory: `~`
    * The path of the target application's parent directory: `{{APP_DIR}}` 
    * The path of the target application's executable: `{{EXEC}}`
    * The results output directory: `{{RESULTS_DIR}}`

We do not substitute the device family and version information into the Subliminal 
trace template itself because it is not clear from the trace template format what 
exactly we would substitute to determine the configuration used by Instruments.

Each template is saved as XML to avoid an unnecessary conversion step before 
substitution, and without the ".tracetemplate" extension so that it may be 
stored in `~/Library/Application Support/Instruments/Templates` without appearing 
as a viable template in Instruments' "New File" dialog.
