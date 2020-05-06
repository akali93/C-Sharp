using System;
using System.IO;
using System.Numerics;

namespace AdobeTrialUpdater
{
    class Program
    {
        static void Main(string[] args)
        {
            // Get the Path where Adobe is Installed
            string defaultPath = Environment.GetFolderPath(Environment.SpecialFolder.ProgramFiles) + @"\Adobe";
            
            // Get all installed Programs
            string[] dirsArr = Directory.GetDirectories(defaultPath);

            foreach (string folderName in dirsArr)
            {
                // Get the file Path
                // on Windows seems that it ignores lower or upper case amt AMT
                string fileXML = folderName + @"\AMT\application.xml";
                
                if (File.Exists(fileXML))
                {
                    // Check if file Exists
                    Console.WriteLine(folderName);
                    Console.WriteLine("[#] File Status: Ok");

                    // Save the current file in a string
                    string applicationDOTxml = File.ReadAllText(fileXML);

                    // Get the Current TrialSerialNumber 
                    int pFrom = applicationDOTxml.IndexOf("<Data key=\"TrialSerialNumber\">") + "<Data key=\"TrialSerialNumber\">".Length;
                    int pTo = applicationDOTxml.LastIndexOf("</Data></Other>");
                    String oldTrialSerialNumber = applicationDOTxml.Substring(pFrom, pTo - pFrom);
                    
                    Console.WriteLine("[#] Current Licence: " + oldTrialSerialNumber);

                    //Increase current Licence by 1
                    BigInteger increased = BigInteger.Parse(oldTrialSerialNumber) + 1;
                    string newTrialSerialNumber = increased.ToString();
                    Console.WriteLine("[#] Updated Licence: " + newTrialSerialNumber);

                    try {
                        //Replace old TrialSerialNumber with new One
                        applicationDOTxml = applicationDOTxml.Replace(oldTrialSerialNumber, newTrialSerialNumber);

                        //Trying to Replace the file 
                        // Not working currently due to UnauthorizedAccessException
                        File.WriteAllText(fileXML, applicationDOTxml);

                        Console.WriteLine();

                    } 
                        catch (Exception ex)
                    {
                        
                        if (ex is UnauthorizedAccessException) {
                            Console.WriteLine("[#] Access denied");
                            Console.WriteLine();

                            //Get Program Name
                            string dirName = new DirectoryInfo(folderName).Name;

                            // Creates a File on the Desktop for the Program without Access
                            File.WriteAllText(Environment.GetFolderPath(Environment.SpecialFolder.Desktop) + @"\" + dirName + "_application.xml", applicationDOTxml);
                        }
 
                    }

                } 
                    else
                {
                    Console.WriteLine(folderName);
                    Console.WriteLine("[#] File Status: Not found");
                    Console.WriteLine();
                }
            }

            //End
            Console.Read();
        }
    }
}
