import java.io.*;

import javax.ws.rs.*;
import javax.ws.rs.core.*;
import javax.ws.rs.core.Response.*;
import javax.ws.rs.ext.*;

import com.google.common.base.*;

@Provider
public class MyExceptionMapper implements ExceptionMapper<Exception> {
  public Response toResponse(Exception e) {
    StringWriter stringWriter = new StringWriter();
    PrintWriter writer = new PrintWriter(stringWriter, true);
    if (e instanceof WebApplicationException) {
      Response response = ((WebApplicationException) e).getResponse();
      Status status = Status.fromStatusCode(response.getStatus());
      if (status == null)
        writer.println(response.getStatus());
      else
        writer.println(String.format("%s %s", status.getStatusCode(), status.getReasonPhrase()));
      return Response.fromResponse(response).type("text/plain").entity(stringWriter.toString()).build();
    }
    int status = Throwables.getRootCause(e) instanceof RuntimeException ? 400 : 500;
    if (status == 400)
      writer.println(e.getMessage());
    else
      writer.println(Throwables.getStackTraceAsString(e));
    if (status==500)
      e.printStackTrace();
    return Response.status(status).type("text/plain").entity(stringWriter.toString()).build();
  }
}
