# Personas

## This page provides link to guidance for common data consumer types.

### Please click on one of the boxes below to ge more insight for each persona type.

<centre> 
<div
  style={{
    display: 'grid',
    gridTemplateColumns: '1fr 1fr',
    justifyItems: 'stretch',
  }}
>
  {[
    {
      href: "/usecases/not-connected.md",
      src: "https://github.com/user-attachments/assets/ec21157a-55a5-4313-80f6-4071bd09e4c8",
      alt: "Not Connected Persona",
    },
    {
      href: "/usecases/connected.md",
      src: "https://github.com/user-attachments/assets/7848665e-8559-4ded-9554-28c19d556027",
      alt: "Connected Persona",
    },
    {
      href: "/usecases/collaborator.md",
      src: "https://github.com/user-attachments/assets/3f8ae500-c899-4e98-a965-a440b6bfa494",
      alt: "Collaborator Persona",
    },
    {
      href: "/usecases/datauser.md",
      src: "https://github.com/user-attachments/assets/cb79f0b6-6aa2-4e6c-afc4-7e3ffc5a8338",
      alt: "Data User Persona",
    },
  ].map(({ href, src, alt }) => (
    <a
      key={href}
      href={href}
      style={{
        display: 'block',
        height: '250px',
        width: '100%',
        overflow: 'hidden',
      }}
    >
      <img
        src={src}
        alt={alt}
        style={{
          width: '100%',
          height: '100%',
          objectFit: 'cover',
          margin: 0,
          display: 'block',
        }}
      />
    </a>
  ))}
</div>

</centre>
