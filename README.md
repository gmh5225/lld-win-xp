# lld-win-xp
 Make lld to support win-xp

```C++
1.lld\COFF\Config.h
// Chang the value to 4 to support win-xp
  uint32_t majorOSVersion = 4;
// Chang the value to 4 to support win-xp
  uint32_t majorSubsystemVersion = 4;
```

```C++
2.lld\COFF\DriverUtils.cpp
static std::string createDefaultXml() {
  std::string ret;
  raw_string_ostream os(ret);

  // Emit the XML. Note that we do *not* verify that the XML attributes are
  // syntactically correct. This is intentional for link.exe compatibility.
  // Use microsoft xml to support win-xp.
  os << "<?xml version='1.0' encoding='UTF-8' standalone='yes'?>\n"
     << "<assembly xmlns='urn:schemas-microsoft-com:asm.v1' "
        "manifestVersion='1.0'>\n";
  if (config->manifestUAC) {
    os << "  <trustInfo xmlns=\"urn:schemas-microsoft-com:asm.v3\">\n"
       << "    <security>\n"
       << "      <requestedPrivileges>\n"
       << "         <requestedExecutionLevel level=" << config->manifestLevel
       << " uiAccess=" << config->manifestUIAccess << "/>\n"
       << "      </requestedPrivileges>\n"
       << "    </security>\n"
       << "  </trustInfo>\n";
  }
  for (auto manifestDependency : config->manifestDependencies) {
    os << "  <dependency>\n"
       << "    <dependentAssembly>\n"
       << "      <assemblyIdentity " << manifestDependency << " />\n"
       << "    </dependentAssembly>\n"
       << "  </dependency>\n";
  }
  os << "</assembly>\n";
  return os.str();
}
```

